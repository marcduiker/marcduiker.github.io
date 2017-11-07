---
layout: post
title: "Azure Durable Functions - Stateful function orchestrations (part 1)"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/11/05/DurableFunctionsDemo.png" alt="HttpStart DurableFunction">

## Durable Functions

Since my [Getting started with Serverless Architectures using Azure Functions]({{ site.url}}/2017/10/19/serverless-architectures-using-azure-functions.html) session at Techdays I've been closely monitoring the latest news about Azure Functions. The most recent addition is called [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview){:target="_blank"}. With this extension long running and stateful function orchestrations can be developed. This is a welcome addition to the Azure serverless product suite since it is now much easier to implement function chaining and fan-in/fan-out messaging scenarios.

<!--more-->

### How does it work?

Durable Functions are built on top of the [Durable Task Framework](https://github.com/Azure/durabletask){:target="_blank"} which enables development of long running persistent workflows by using a pattern called [Event Sourcing](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview#the-technology){:target="_blank"}. This pattern ensures that all actions in the orchestration function are stored and can be replayed. One of the benefits of this approach is that when the orchestration function instance has triggered another function to perform a task, the orchestration function itself can hibernate (and will not cost anything when the consumption plan is used) until the other function returns its result. 

Durable Functions use Azure Storage queues, tables and blobs to manage state and messages. It uses the storage account which is created when you create a new Function App through the Azure portal.

Lets look at the two most important classes in the Durable Functions family; `Durable​Orchestration​Client` & `Durable​Orchestration​Context`. 

#### Durable​Orchestration​Client

The `Durable​Orchestration​Client` class manages orchestration function instances. 
In order to use this client the following input binding needs to be added:
 
`[OrchestrationClient]DurableOrchestrationClient orchestrationClient`

The client exposes the following functionalities: 

##### Starting an instance 

This will start a new instance of an orchestration function.

`string instanceId = await orchestrationClient.StartNewAsync(functionName, eventData);`

This [Gist](https://gist.github.com/marcduiker/e0e7cb5c7eb81614ab5a4470de95d74a){:target="_blank"} shows an HttpTrigger function which can start orchestration functions using the `orchestration/{functionName}` route of the Function App:

{% gist e0e7cb5c7eb81614ab5a4470de95d74a %}

##### Stopping an instance

The orchestration function instance can be terminated without waiting for the results from other functions triggered by the orchestration function instance.

`await orchestrationClient.TerminateAsync(instanceId, terminationReason);`

##### Retrieving the status of an instance

Since Durable Functions are meant to be long running it is useful to query the status of the orchestration:

`var status = await orchestrationClient.GetStatusAsync(instanceId);`

The returning type is `DurableOrchestrationStatus` which contains a `RuntimeStatus` property indicating if the orchestration function instance is still running, completed or terminated.

##### Raising events

The orchestration client is also capable of raising events:

`await orchestrationClient.RaiseEventAsync(instanceId, eventName, eventData);`

These events can be picked up by other functions referenced in the orchestration function by using the `Durable​Orchestration​Context`.

#### Durable​Orchestration​Context

The `DurableOrchestrationContext` class is used to call & schedule other functions, sub-orchestrations or wait for events. 
The orchestration function requires the following input binding:

`[OrchestrationTrigger]DurableOrchestrationContext orchestrationContext`
 
This [Gist](https://gist.github.com/marcduiker/e127deda371260f71ca93b1182e35a85){:target="_blank"} shows the most basic orchestration function, which actually doesn't do any orchestration, it only retrieves input from the context:

{% gist e127deda371260f71ca93b1182e35a85 %}

Although the `DurableOrchestrationContext` class contains about 15 methods I'll only describe a few of them in this initial post.

##### Get function input

When input is passed to the orchestration function (as JSON) this can be retrieved & deserialized by the `GetInput<T>` method. 

To retrieve a list of names the following can be used:

`var names = orchestrationContext.GetInput<List<string>>()`

##### Call another function

The orchestration function can call other functions using the `CallActivityAsync<T>` method.

The following example calls the _CollectNames_ function. Passes an initial list of names (_names_) and returns a Task of type `List<string>`:

`var collectNamesResult = await context.CallActivityAsync<List<string>>("CollectNames", names);`

##### Wait for an event

As mentioned earlier, the `OrchestrationClient` can raise events and other functions can react on these by using the `WaitForExternalEvent<T>` method.

The following example describes the method to wait on the _addname_ event which has a return type of `string`:

`var addNameResult = await orchestrationContext.WaitForExternalEvent<string>("addname");`

When it's required to await several events use the `await Task.WhenAll(...)` or `Task.WhenAny(...)` methods.

##### Restart the orchestration

When an orchestration is required to be running forever and its full history is not of importance the `ContinueAsNew(object input)` method can be used to restart the orchestration which resets its history. The current state of orchestration can still be kept because it can be passed to this method and used again at the start of the orchestration using `GetInput<T>`.

This example restarts the orchestration function and passing it a list of strings (names):

`orchestrationContext.ContinueAsNew(names);`

The Durable Functions documentation shows how this can be used in [Eternal Orchestrations](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-eternal-orchestrations){:target="_blank"}.

### <a id="developing"></a>Developing Durable Functions

I recommend creating compiled orchestration functions using Visual Studio 2017 because currently durable functions can only be developed in C# (support for other languages will follow).

The following tools/packages are required:

- The [Microsoft Azure Storage Emulator](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator){:target="_blank"} is a standalone tool which uses SQL Server LocalDB and the local file storage instead of Azure Storage. The emulator needs to be started before you can run & debug Durable Functions locally. 
- The [Azure Functions and Web Jobs Tools](https://docs.microsoft.com/en-us/azure/azure-functions/functions-develop-vs){:target="_blank"} Visual Studio extension. This extension adds a project template to Visual Studio to create Function Apps and run/debug them locally.
- In your Function App you need a reference to this NuGet package: `Microsoft.Azure.WebJobs.Extensions.DurableTask` (currently 1.0.0-beta). 

I had some issues while adding this package since it has a dependency on `Microsoft.Azure.WebJobs` __2.1.0-beta4__ while the Function App project template uses __2.1.0-beta1__. Make sure when you create a new Function App using the project template you update `Microsoft.NET.Sdk.Functions` NuGet package to the most recent one (now 1.0.6) so the `Microsoft.Azure.WebJobs` versions match up.

Finally make sure you have the following local connection strings in your local.settings.json in your Function App:

{% gist 1099547c20666cf179f9590e47b978ff %}  

### Next steps

I've now spent a couple of days tinkering with Durable Functions and I have to say that I enjoy this framework a lot. It's more powerful than I imagined and although I was a bit skeptical about a more direct coupling of functions by using these orchestration functions I definitely see their value.

In next posts I'll share more examples about the `DurableOrchestrationContext` and include a demo about the orchestration function I wrote.

__[Continue with Part 2]({{ site.url}}/2017/11/07/durable-azure-functions-stateful-orchestrations-part2.html)__