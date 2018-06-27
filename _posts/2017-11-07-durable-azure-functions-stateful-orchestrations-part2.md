---
layout: post
title: "Azure Durable Functions - Stateful function orchestrations (part 2)"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/11/05/DurableFunctionsDemo.png" alt="HttpStart DurableFunction">

## Durable Function Walkthrough

In my [previous post]({{ site.url}}/2017/11/05/durable-azure-functions-stateful-orchestrations.html) I gave an introduction on Durable Functions, an extension on Azure Functions which can be used to write stateful and long-running orchestration functions.

In this post we'll look into more detail at the `HttpStart` and `HelloWorld` functions from the previous post. We'll run them locally by triggering them using [Postman](https://www.getpostman.com/){:target="_blank"} and looking at the responses.

<!--more-->

### Setup

You can start by cloning my [demos-azure-durable-functions](https://github.com/marcduiker/demos-azure-durable-functions.git){:target="_blank"} GitHub repository. This repo holds the `DurableFunctionsDemo` Function App which contains the following functions:

- `HttpStart`, the HttpTrigger function which starts an orchestration function.
- `HelloWorld`, the most basic orchestration function ever.
- `CollectNames`, an 'eternal' orchestration function which waits for external events (I'll cover this one in the next blog post).
 
Have a look at the [Developing Durable Function]({{ site.url}}/2017/11/05/durable-azure-functions-stateful-orchestrations.html#developing) section from my previous post in order to make sure you have all the required components to run the functions locally.

### Running Durable Functions Locally

I usually type _azure_ in the Windows desktop search box to find the Microsoft Azure Storage Emulator and start it from there:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/azurestorageemulator-started.png" alt="Azure Storage Emulator is started">

Open the `DurableFunctionsDemo.sln` solution and press _F5_ to run it locally:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/functionsruntime1.png" alt="Local Functions Runtime starting">

After a few seconds you'll see the local endpoint of the `HttpStart` function in green:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/functionsruntime2.png" alt="Local Functions Runtime is up and running with local endpoint">

You can copy & paste this url into Postman (or any other HTTP API test client) and change the following:
   - Make this a __POST__ request
   - Change __{functionName}__ to __HelloWorld__
   - In the Body tab, select __raw__ and __JSON (application/json)__ as the content-type.
   - Type a string in the body of the request, such as __"Durable Functions!"__.

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/postman-helloworld-request.png" alt="Request to orchestration/HelloWorld">

Click _Send_ to do the request.

You might expect to see __"Hello Durable Functions!"__ in the response body but that is not the case. You'll see this instead:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/postman-helloworld-response.png" alt="Reponse from orchestration/HelloWorld">

This is because you receive the response from the `HttpStart` function (the `DurableOrchestrationClient`) and not the `HelloWorld` function directly.

The `DurableOrchestrationClient` class exposes the `CreateCheckStatusResponse` API which generates an HTTP response containing the HTTP API methods that the client supports. In this response we see the following API methods:
- `statusQueryGetUri`; when a GET request is made to this endpoint the status of the orchestration function is returned (a serialized `Durable​Orchestration​Status`). 
- `sendEventPostUri`; when a POST request is made to this endpoint (including a valid event name and event data) an event is triggered which can be picked up by an orchestration function. I'll come back to this in the next blog post. 
- `terminatePostUri`; when a POST request is made to this endpoint the orchestration function is stopped without waiting for normal completion.

The `id` in the response is the `InstanceId` of the orchestration function.

When you click the `statusQueryGetUri` endpoint and send it as a GET request you'll get the following response:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/postman-helloworld-getstatusqueryuri.png" alt="Request and response to the statusQueryGetUri endpoint">

Now you have the input & output of the `HelloWorld` orchestration function and the status of the function.

Sending requests to the `sendEventPostUri` and `terminatePostUri` endpoints don't make sense in this orchestration function since it's not listening to external events and it is completed quite fast so terminating it before it reaches completed state is difficult. I'll get into these two methods in later posts.

#### Looking under the hood

Finally, if you want to see where the local orchestration function history is kept, (which is used by the Event Sourcing pattern) you can have a look using the [Microsoft Azure Storage Explorer](https://azure.microsoft.com/en-us/features/storage-explorer/){:target="_blank"} tool. This is a great tool when you're working with any type of Azure Storage, including local emulations.

When you navigate to (Local and Attached) > Storage Accounts > Development you'll see several blob containers, queues and tables which start with __durablefunctionshub__. If you look at the `DurableFunctionsHubHistory` table you can see all executions of the orchestration functions including the their input and output: 

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/11/07/storageexplorer-table.png" alt="DurableFunctionsHubHistory table">

### Next steps

At this moment you should have gained some insights how to run & debug Durable Functions locally. In the next post I'll demonstrate an 'eternal' orchestration function which waits for external events and is stateful.