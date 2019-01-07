---
layout: post
title: "Durable Functions API - DurableOrchestrationClient (part 1)"
tags: azure durable functions serverless faas stateful orchestration
---

## DurableOrchestrationClient(Base) class (Part 1)

This is the first part of a series of blogs/vlogs where I'll talk about the Durable Functions API.

In the video linked below I'm covering methods from the  `DurableOrchestrationClient`(`Base`) class on how to start a new orchestration and how to retrieve the status and the result of the orchestration:

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
- Microsoft docs: [Optional uri paramters and default values](https://docs.microsoft.com/en-us/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#optional-uri-parameters-and-default-values).

### Links to other post for this series

- DurableOrchestrationClient(Base) class (Part 1)
