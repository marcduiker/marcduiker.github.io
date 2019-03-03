---
layout: post
title: "Creating the Azure Functions Updates Twitterbot"
tags: azure durable functions serverless faas stateful orchestration twitter github
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/azfuncupdates_twitter.png" alt="Azure Funtions Updates Twitter account screenshot">

## TL;DR

Go to [https://twitter.com/az_func_updates](https://twitter.com/az_func_updates) and follow that account to stay up to date with new Azure Functions releases!

<!--more-->

## What I'm trying to solve

The [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/) ecosystem consists of a lot of moving parts. There's the runtime, core tools, VS templates, Webjobs SDK and its dozen or so extensions, definitions for writing functions using TypeScript and much more.

As a developer, it's important you are aware of the latest releases and what is compatible with one another. I've spent quite some time troubleshooting incompatible runtimes and packages in the past, and it would be great if this experience can be improved.

## The idea

I was thinking about a way to notify developers about new releases related to Azure Functions. Almost all of the Azure Functions components are on GitHub so I can use the [GitHub API](https://developer.github.com/) to check for latest releases. 

Since the IT community is very active on Twitter that can be the communication channel, so the idea of an [Azure Functions Updates Twitterbot](https://twitter.com/az_func_updates) was born.

Surely this does not solve the issue of dealing with incompatible components, but it does help a bit in spreading the information about new releases and knowing when to update which exact piece of the puzzle.

## The implementation

Since I'm a big fan of Azure Functions myself, I wanted to use this service as the backend for the Twitterbot. Logic Apps could also have been a valid option for a part of it, but I prefer to have local debugging and testing which is easily done with Azure Functions.

The design of the application is as follows:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/azfunctionupdates_diagram.png" alt="Azure Functions Updates component diagram.">

### Storage: Azure Table Storage

The application needs to store two things:

1. The GitHub repositories to check for new releases.
2. The latest release for each of the repositories to compare against. 

Both require very little storage. I decided on using Azure Table Storage since that is easy to setup &amp; use and it is also very performant.

The repositories to check are stored in the `RepositoryConfigurations` table:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/repository_configurations.png" alt="Azure Storage Table with repository configurations.">

The release information retrieved from GitHub is stored in the `Releases` table:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/releases.png" alt="Azure Storage Table with release info from GitHub.">

I'm not storing the entire GitHub Release object, only the properties I need (or plan to use soon).

### Compute: Azure Functions

As mentioned before I'm using Azure Functions for the compute part of this application. In specific I'm using the [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview) extension to control the flow of the application. A timer trigger starts the [`ReleaseUpdateOrchestration`](https://github.com/marcduiker/az-func-updates/blob/master/src/AzureFunctionsUpdates/Orchestrations/ReleaseUpdateOrchestration.cs) function each hour.

The flow (in pseudo code) is as follows:
```
- GetRepositoryConfigurations
- for each of the repositories:
    - GetLatestReleaseFromGitHub
    - GetLatestReleaseFromHistory
- for each of the repositories:
    - if release from GitHub is not equal to release from History
        - SaveLatestRelease
        - PostUpdate
```
The orchestrator function uses the fan-out/fan-in pattern since it can call the same activity functions for each of the configured repositories in parallel.

### GitHub API

For the GitHub integration, I'm using [Octokit](https://github.com/octokit/octokit.net), an excellent GitHub API client library for .NET.

The usage is the following:
{% gist 50fb218fb33afd8e3c2447306a3af7d2 %}

I'm doing unauthenticated requests which means the application is [rate limited at 60 requests per hour](https://developer.github.com/v3/#rate-limiting). I'm far from that number of repositories to check, but it's good to be aware this doesn't scale very well. For authenticated requests, the rate limiting is set much higher, at 5000 requests per hour. 

### Twitter API

To have an application post tweets, you need a Twitter developer account and set up a Twitter application. There is quite some documentation about this (almost too much), but it's quite easy to follow when you go through the [getting started docs](https://developer.twitter.com/en/docs/basics/getting-started). You basically need to fill in a couple of online forms and promise you won't do any evil with you Twitterbot.

To post the updates to Twitter from my function app I'm using [Tweetinvi](https://github.com/linvi/tweetinvi). I found this Twitter client library for .NET Core very easy to use:

{% gist 9c1266cdc2f6998e67dcf911928b67ef %}

## Overall development experience

I built this application in about 6 evenings over the last month. I find it quite amazing that thanks to the work of others, who have built these cloud services and client libraries, I can make something like this in such little time.

Development was mostly done in Visual Studio 2019 Preview and a bit of VS Code. I've set up a CI/CD pipeline in Azure DevOps so as soon as I merge to master a new release is built and deployed to Azure.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/azuredevops.png" alt="Azure Devops CD pipeline.">

It was certainly an enjoyable experience to make this, and I hope you will benefit from it. 

I already have plans to extend the application beyond GitHub release updates. So in time, there will be some significant upgrades.

## Resources &amp; feedback

The full source code of the Azure Functions Updates Twitterbot is found in the [az-func-updates](https://github.com/marcduiker/az-func-updates) repo on GitHub.

If you have feature requests or suggestions, feel free to add those as an [issue](https://github.com/marcduiker/az-func-updates/issues), so we can discuss it.
