---
layout: post
title: "Discovering the Durable Functions API - Starting orchestrations (DurableOrchestrationClient part 1)"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/01/07/DurableOrchestrationClientBase1_900.png" alt="Methods in DurableOrchestrationClientBase">

## DurableOrchestrationClient(Base) class - Starting & waiting for completion

This post is the first part of a series of blogs/vlogs to discover the Durable Functions API.

<!--more-->

In the video linked below, I'm looking into methods from the  [`DurableOrchestrationClient`(`Base`)](https://github.com/Azure/azure-functions-durable-extension/blob/master/src/WebJobs.Extensions.DurableTask/DurableOrchestrationClientBase.cs) class on how to start a new orchestration instance and how to retrieve the status and the result of the instance:

- `StartNewAsync(string orchestratorFunctionName, object input)`
- `StartNewAsync(string orchestratorFunctionName, string instanceId, object input)`
- `CreateCheckStatusResponse(HttpRequestMessage request, string instanceId)`
- `WaitForCompletionOrCreateCheckStatusResponseAsync(HttpRequestMessage request, string instanceId)`
- `WaitForCompletionOrCreateCheckStatusResponseAsync(HttpRequestMessage request, string instanceId, TimeSpan timeout)`
- `WaitForCompletionOrCreateCheckStatusResponseAsync(HttpRequestMessage request, string instanceId, TimeSpan timeout, TimeSpan retryInterval)`

Here's the video, please give it a thumbs up if you like it and subscribe to my channel so you'll be notified of new videos:

<iframe width="560" height="315" src="https://www.youtube.com/embed/mRDesdK3W8Q" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Resources

- GitHub repo: [github.com/marcduiker/demos-azure-durable-functions](https://github.com/marcduiker/demos-azure-durable-functions).
- Microsoft docs: [Durable Functions HTTP API](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-http-api).
- Microsoft docs: [Optional uri parameters and default values](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#optional-uri-parameters-and-default-values).

### Links to other posts in this series

- Starting orchestrations (DurableOrchestrationClient Part 1)
- [Retrieving the orchestration status (DurableOrchestrationClient Part 2)](/2019/02/17/durable-functions-api-durableorchestrationclient-2.html)
- [Purge & Terminate Orchestrations (DurableOrchestrationClient Part 3)](/2019/08/12/durable-functions-api-purge-terminate.html)
