---
layout: post
title: "Creating the Azure Functions Updates Twitterbot"
tags: azure durable functions serverless faas stateful orchestration twitter github
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/azfuncupdates_twitter.png" alt="Azure Funtions Updates Twitter account screenshot">

## TL;DR

Go to [https://twitter.com/az_func_updates](https://twitter.com/az_func_updates) and follow that account in order to stay up to date with new Azure Functions releases!

<!--more-->

## What I'm trying to solve

The [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/) ecosystem consists of a lot of moving parts. There's the runtime, core tools, VS templates, Webjobs SDK and it's dozen or so extensions, and type definitions for writing functions using TypeScript.

As a developer it's important you are aware of the latest releases and what is compatible with one another. I've spent quite some time troubleshooting incompatible runtimes and packages in the past and it would be great if this experience can be improved.

## The idea

I was thinking about a way to notify developers about new releases related to Azure Functions. Almost all of the Azure Functions components are on GitHub so I can use the [GitHub API](https://developer.github.com/) to check for latest releases. 

Since the IT community is very active on Twitter that can be the communication channel. So a new idea of an [Azure Functions Updates twitterbot](https://twitter.com/az_func_updates) was born.

Surely this does not solve the issue of having incompatible components but it does help a bit in spreading the information about new releases.

## The implementation

Since I'm a big fan of Azure Functions myself I wanted to use this service as the backend for the twitterbot. Logic Apps could also have been a valid option for a part of it but I prefer to have local debugging and testing with Azure Functions.

The design of the application is as follows:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/azfunctionupdates_diagram" alt="Azure Functions Updates component diagram.">

### Storage: Azure Table Storage

The application needs to store two things:

- The GitHub repositories to check for new releases.
- The latest release for each of the repositories to compare against. 

Both require very little storage. I decided on using Azure Table Storage since that is easy to setup &amp; use and it is also very performant.

The repositories to check are stored in the `RepositoryConfigurations` table:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/repository_configurations.png" alt="Azure Storage Table with repository configurations.">

The release information retrieved from GitHub is stored in the `Releases` table:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2019/03/03/releases.png" alt="Azure Storage Table with release info from GitHub.">

I'm not storing the entire GitHub Release object, only the properties I need (or plan to use soon).

### Compute: Azure Functions

As mentioned before I'm using Azure Functions as the compute part of this application. In specific I'm using the [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview) extension to control the flow of the application. A timer trigger starts the [`ReleaseUpdateOrchestration`](https://github.com/marcduiker/az-func-updates/blob/master/src/AzureFunctionsUpdates/Orchestrations/ReleaseUpdateOrchestration.cs) function each hour.

The flow is as follows:
- GetRepositoryConfigurations
- for each of the repositories:
    - GetLatestReleaseFromGitHub
    - GetLatestReleaseFromHistory
- for each of the repositories:
    - if release from GitHub is not equal to release from History
        - SaveLatestRelease
        - PostUpdate

The orchestrator function uses the fan-out/fan-in pattern since it can call the same activity functions for each of the configured repositories in parallel.

#### GitHub API

For the GitHub integration I'm using [Octokit](https://github.com/octokit/octokit.net), an excellent GitHub API client library for .NET.

The usage is the following:
```csharp
var client = new GitHubClient(new ProductHeaderValue("SomeApplicationIdentifier"));

var latestRelease = await client.Repository.Release.GetLatest(repoConfiguration.RepositoryOwner, repoConfiguration.RepositoryName);
```

I'm doing unauthenticated requests which means the application is [rate limited at 60 requests per hour](https://developer.github.com/v3/#rate-limiting). I'm far from that number of repositories to check but it's good to be aware this doesn't scale very well. For authenticated requests the rate limiting is set much higher, at 5000 requests per hour. 

#### Twitter API

In order to have an application post tweets you need a Twitter developer account and setup a Twitter application. There is a lot of documention about this (almost too much), but it's quite easy to follow when you go through the [getting started docs](https://developer.twitter.com/en/docs/basics/getting-started).

To post the updates to Twitter from my function app I'm using [Tweetinvi](https://github.com/linvi/tweetinvi). I found this Twitter client library for .NET Core very easy to use.

```csharp
 var creds = new TwitterCredentials(consumerApiKey, consumerApiSecret, accessToken, accessTokenSecret);
var tweetMessage = CreateMessage(newRelease);

var tweet = Auth.ExecuteOperationWithCredentials(creds, () =>
{
    return Tweet.PublishTweet(tweetMessage);
});
```

## Overall development experience

I built this application in about 6 evenings over the last month. I find it quite amazing that thanks to the work of other people, who have built these cloud services and client libraries, I can make something like this in such little time.

Development was mostly done in Visual Studio and a bit of VS Code. There's a CI/CD pipeline setup in Azure DevOps so as soon as I merge to master a new release is deployed to Azure.

## Future work

## Resources &amp; feedback

The full souce code of the Azure Functions Updated Twitterbot can be found in the [az-func-updates](https://github.com/marcduiker/az-func-updates)  repo on GitHub.

If you feature requests or suggestions, feel free to add those as an [issue](https://github.com/marcduiker/az-func-updates/issues) so we can discuss it.
