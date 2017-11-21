---
layout: post
title: "Azure Functions Tips: Grouping Functions into Function Apps"
tags: azure functions faas serverless deployment
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/11/21/serverless-architecture.png" alt="Simple serverless architecure example">

## Guidelines for Function Apps

Earlier today I read a tweet where a developer wasn't sure when to group several Azure Functions in one Function App. The Azure Function engineers responded swiftly and they'll extend the official docs with some guidelines in this area soon. I think this is a really interesting topic so let's start with a few ideas of my own.

<!--more-->

Azure Functions are hosted in a Function App. The following is written in the [Azure documentation](https://docs.microsoft.com/en-us/azure/azure-functions/functions-reference#function-app){:target="_blank"} about a Function App:

_"A function app is comprised of one or more individual functions that are managed together by Azure App Service. All of the functions in a function app share the same pricing plan, continuous deployment and runtime version. Functions written in multiple languages can all share the same function app. Think of a function app as a way to organize and collectively manage your functions."_

Sofar so good but should you put all your functions inside one Function App? Or should you put each function inside its own Function App? 

You're now dealing with a serverless architecture challenge. Let's look at some of the aspects involved in order to make an informed decision how to group your functions.

### Serverless Architecture Aspects

Here are a couple of aspects which play a big role in deciding a serverless architecture:

- Single responsibility principle
- Workload distribution
- DevOps practices
- Resilience

### Single Responsibility Principle

I hope this principle is nothing new, the 'S' in SOLID. As quoted from [Wikipedia]((https://en.wikipedia.org/wiki/Single_responsibility_principle){:target="_blank"}):

_"The single responsibility principle is a computer programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class."_

In my opinion in this serverless era, a grouping of functions (the Function App) is the equivalent of a module or class as described in the quote.

I somewhere heard or read the following related to this principle _"What changes together should live together"_. I adjusted this a bit and prefer __"What changes together should be deployed together"__. So if a new business functionality requires a change of two functions it means those two functions should exist in the same Function App.

### Workload distribution

The workload of your functions also determine how they should be grouped. Let's consider a scenario where you have three functions: A, B, and C. The workload across all of them is evenly distributed and the memory consumption across all of them is below the maximum available memory of the Function App (currently 1.5 GB on the consumption plan). In this case you could run all your functions in the same Function App.

Now let's consider another scenario: function A receives a much higher workload than B and C and requires a great deal of the available memory. In this case you should consider moving function A to a separate Function App so it can scale independently when the workload is very high. The low workload functions, B and C, can remain in another Function App and won't be impacted now by function A.

### DevOps

If you have multiple teams developing, deploying & testing functions then it makes sense to have each team take responsibility of a set of functions in one or more Function Apps. Each team can work independently on their own Function Apps and deployments across teams do not even need to be synced if functions are loosely coupled (e.g. using message queues and agreed interfaces).

Side note: I also suggest having a code repository per Function App so you can get up and running quickly using the built in [continuous deployment](https://docs.microsoft.com/en-us/azure/azure-functions/functions-continuous-deployment){:target="_blank"} options. It's not as complete as a full CI/CD pipeline in VSTS but it's definitely better than nothing.

### Resilience

Downtime is never a good thing right? So when you design your serverless architecture you need to take into account that some components will eventually fail. The App Service can have a hick-up or some 'bad code' has somehow got through your CI/CD pipeline and now runs in production. To minimize the impact of the issue you should consider splitting up your functions in several Function Apps to make them __independently deployable__ and therefore __independently failable__.

Again I suggest to use messages queues between functions (either in a separate storage account or using Azure Service Bus) so the state is not lost when a Function App goes down (either through failure or regular deployment). As an alternative [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview){:target="_blank"} offers a built-in mechanism for storing state and looks really promising.

### Conclusion: It depends

There is no single answer how to group your functions into Function Apps. But as can be concluded from the described aspects (which are far from exhaustive) it is usually a good idea to keep your Function App lean with only a few closely related functions.