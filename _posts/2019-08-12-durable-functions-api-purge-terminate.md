---
layout: post
title: "Discovering the Durable Functions API - Purge & Terminate Orchestrations (DurableOrchestrationClient part 3)"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/08/12/purge-terminate-cover.png" alt="Methods in DurableOrchestrationClientBase">

## DurableOrchestrationClient(Base) class - Purge & Terminate

This post is the third part of a series of blogs/vlogs to discover the Durable Functions API.

<!--more-->

In the video linked below, I'm looking into the functionality from the [`DurableOrchestrationClient`(`Base`)](https://github.com/Azure/azure-functions-durable-extension/blob/master/src/WebJobs.Extensions.DurableTask/DurableOrchestrationClientBase.cs) class which is used to:

- Purge the history of orchestration instances
- Terminate a running orchestration

### Purging

Purging the history of orchestration instances means deleting the records from table storage. I recommend doing this regularly, for instance using timer triggered function, so you won't clutter up your storage account with gigabytes of data you likely don't need. I usually remove all completed instances older than a week but keep the failed, canceled and terminated ones a bit longer. Make sure you don't inadvertently purge the running and pending instances!

There are two methods to purge the history of orchestration instances. The first method listed below deletes the history for multiple orchestration instances. The method requires a DateTime range and a collection of `OrchestrationStatus` enum values. These arguments act as a filter, so only the instance history within the DateTime range and selected statuses are purged:

``` csharp
Task<PurgeHistoryResult> PurgeInstanceHistoryAsync(
    DateTime createdTimeFrom, 
    DateTime? createdTimeTo, 
    IEnumerable<OrchestrationStatus> runtimeStatus)
```

The second method deletes the history of a single orchestration instance:

```csharp
Task<PurgeHistoryResult> PurgeInstanceHistoryAsync(
        string instanceId);
```

### Termination

Terminating an orchestration instance means you stop a running orchestration. You only change the state of the instance. The instance history is still available in table storage. Using this method should only be used in exceptional cases. Perhaps you have an orchestration with a bug, so it keeps running forever, and you want it to stop.

The method requires an orchestration instance ID and a reason why you are stopping the instance:

```csharp
Task TerminateAsync(string instanceId, string reason);
```

### Video

Here's the video, please give it a thumbs up if you like it and subscribe to my channel if you haven't done so already:

<iframe width="560" height="315" src="https://www.youtube.com/embed/ePPEcNOzlnk" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

### Resources

- GitHub repo: [github.com/marcduiker/demos-azure-durable-functions](https://github.com/marcduiker/demos-azure-durable-functions).

### Links to other posts in this series

- [Starting Orchestrations (DurableOrchestrationClient Part 1)](/2019/01/07/durable-functions-api-durableorchestrationclient-1.html)
- [Retrieving the Orchestration Status (DurableOrchestrationClient Part 2)](/2019/02/17/durable-functions-api-durableorchestrationclient-2.html)
- Purge & Terminate Orchestrations (DurableOrchestrationClient Part 3)
