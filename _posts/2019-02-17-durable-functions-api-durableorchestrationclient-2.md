---
layout: post
title: "Discovering the Durable Functions API - Retrieving the orchestration status (DurableOrchestrationClient part 2)"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/02/10/DurableOrchestrationClientBase2_900.png" alt="Methods in DurableOrchestrationClientBase">

## DurableOrchestrationClient(Base) class - Retrieving status

This post is the second part of a series of blogs/vlogs to discover the Durable Functions API.

<!--more-->

In the video linked below, I'm looking into the functionality from the  [`DurableOrchestrationClient`(`Base`)](https://github.com/Azure/azure-functions-durable-extension/blob/master/src/WebJobs.Extensions.DurableTask/DurableOrchestrationClientBase.cs) class, which can be used to retrieve the status of orchestration instances.

These are the methods to retrieve the status of __one__ orchestration and return a [`DurableOrchestrationStatus`](https://github.com/Azure/azure-functions-durable-extension/blob/master/src/WebJobs.Extensions.DurableTask/DurableOrchestrationStatus.cs) object: 
- `GetStatusAsync(string instanceId)`
- `GetStatusAsync(string instanceId, bool showHistory);`
- `GetStatusAsync(string instanceId, bool showHistory, bool showHistoryOutput, bool showInput = true);`

These are the methods to retrieve the status for __multiple__ orchestrations and they return an `IList<DurableOrchestrationStatus>`:
- `GetStatusAsync(CancellationToken cancellationToken = default(CancellationToken));`
- `GetStatusAsync(DateTime createdTimeFrom, DateTime? createdTimeTo, IEnumerable<OrchestrationRuntimeStatus> runtimeStatus, CancellationToken cancellationToken = default(CancellationToken));`


Here's the video, please give it a thumbs up if you like it and subscribe to my channel if you haven't done so already:

<iframe width="560" height="315" src="https://www.youtube.com/embed/d5fsidj_EDs" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Resources

- GitHub repo: [github.com/marcduiker/demos-azure-durable-functions](https://github.com/marcduiker/demos-azure-durable-functions).
- Microsoft docs: [Durable Functions HTTP API](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-http-api).

### Links to other posts in this series

- [Starting Orchestrations (DurableOrchestrationClient Part 1)](/2019/01/07/durable-functions-api-durableorchestrationclient-1.html)
- Retrieving the Orchestration Status (DurableOrchestrationClient Part 2)
- [Purge & Terminate Orchestrations (DurableOrchestrationClient Part 3)](/2019/08/12/durable-functions-api-purge-terminate.html)
