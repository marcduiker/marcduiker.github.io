---
layout: post
title: "Durable Functions API - Writing Safe Orchestrations"
tags: azure durable functions serverless faas stateful orchestration
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2020/09/16/safe-orchestrations-cover.png" alt="Slide showing Writing Safe Orchestrations">


This article is part of [#ServerlessSeptember](https://aka.ms/ServerlessSeptember2020). You'll find other helpful articles, detailed tutorials, and videos in this all-things-Serverless content collection. New articles from community members and cloud advocates are published every week from Monday to Thursday through September.

Find out more about how Microsoft Azure enables your Serverless functions at [https://docs.microsoft.com/azure/azure-functions/](https://docs.microsoft.com/azure/azure-functions/?WT.mc_id=servsept20-devto-cxaall).

<!--more-->

## Writing Safe Orchestrations

This post is the fifth part of a series of blogs/vlogs to discover the Durable Functions API.

In the video linked below, I'm diving into the [Durable Task Analyzer](https://github.com/Azure/azure-functions-durable-extension/releases/tag/Analyzer-v0.3.0), which is bundled with the Durable Functions extension. This C# Roslyn analyzer helps you to write deterministic code for your orchestrators, so it's safe to be replayed. The analyzer detects code violations which are described on [this docs page](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-code-constraints).

### Video

Here's the video, please give it a thumbs up if you like it and please subscribe to my channel if you haven't done so already:

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZtIQgR25_Y0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>


### Resources

- GitHub repo with demo solution containing the code violations & workarounds: [github.com/marcduiker/demos-durable-task-analyzer](https://github.com/marcduiker/demos-durable-task-analyzer).
- GitHub repo with Durable Functions code snippets: [github.com/marcduiker/durable-functions-snippets](https://github.com/marcduiker/durable-functions-snippets).

### Links to other posts in this series

- [Starting Orchestrations (DurableOrchestrationClient Part 1)](/2019/01/07/durable-functions-api-durableorchestrationclient-1.html)
- [Retrieving the Orchestration Status (DurableOrchestrationClient Part 2)](/2019/02/17/durable-functions-api-durableorchestrationclient-2.html)
- [Purge & Terminate Orchestrations (DurableOrchestrationClient Part 3)](/2019/08/12/durable-functions-api-purge-terminate.html)
- [Human Interaction Pattern (DurableOrchestrationClient Part 4)](/2020/03/15/durable-functions-api-durableorchestrationclient-4.html)