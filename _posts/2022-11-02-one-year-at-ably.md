---
layout: post
title: One year at Ably as a Developer Advocate
tags: 
---

It has been one year since I've joined the DevRel team at Ably as Sr Developer Advocate. In this post I'll highlight some things I've been working on and what I've learned in the past year. I'll cover content creation, developer tooling, events, roadmap & predictability, communication & relationships, and finally 'anything that could be improved'.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/DevRel_team_marc_selected.png" alt="Ably DevRel team">

<!--more-->

## Content creation

Before I joined Ably, I mostly worked on event-driven architectures using .NET-based backends and Azure services (Azure Functions, CosmosDB, Service Bus). Ably really shines when real-time communication is needed between clients, or between servers and clients, so this meant I had to get up to speed with client-side programming, and WebSocket-based communication using the Ably SDK.

During my onboarding, I created my first demo, [Agile Flush](https://agileflush.ably.dev/), a small and playful web app to do remote planning poker. This was my first introduction to VueJS and the Ably client SDK, and after a while I got to like working with Vue. It did take me some time to get used to the toolchain though.

I've stuck to using Vue (TypeScript based, with Vite for the dev tooling) for most of my demos where I need a front-end. It pairs nicely with Azure Static Web Apps that I use for hosting the demos.

Here's a list with all the demos I've created so far, including related content pieces.

### Agile Flush

A playful web app for remote poker planning. The clients publish their votes and actions to Ably, and Ably pushes the updates to the subscribed clients.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/agileflush_screenshot.png" alt="Agile Flush web app">

- [Blog post](https://ably.com/blog/tutorial-vuejs-nodejs-azure-static-web-apps)
- [Video](https://www.youtube.com/watch?v=59BZCQuRRkM)
- [Live demo](https://agileflush.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/agile-flush-vue-app)

<iframe width="560" height="315" src="https://www.youtube.com/embed/59BZCQuRRkM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### What is pub/sub and how to apply it in C# .NET to build a chat app

The demo is a .NET 6 console application that can publish or subscribe to messages which get delivered through Ably. The blog post also covers the basics what pub/sub is, the typical use cases, and benefits.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/console_chat.gif" alt="Console Chat">

- [Blog post](https://ably.com/blog/use-pub-sub-to-build-a-chat-app-with-csharp-net)
- [GitHub repo](https://github.com/ably-labs/pubsub-demo-dotnet)

### Serverless WebSockets Quest

A turn-based mini game with with real-time aspects build on Azure Functions, Durable Functions, and Ably.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/serverless-websockets-play.gif" alt="Serverless WebSockets Quest">

- [Blog post](https://ably.com/blog/quest-for-serverless-websockets-azure-functions-adventure)
- [Video](https://www.youtube.com/watch?v=KHzdc3USFU4)
- [Live demo](https://quest.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/serverless-websockets-quest)

<iframe width="560" height="315" src="https://www.youtube.com/embed/KHzdc3USFU4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Collaborative Pixelart Drawing

A collaborative pixelart drawing canvas. Multiple users draw on a canvas, and their movement and actions are shared with the other connected users.

I created two versions of this demo, one using Ably, and one using Azure Web PubSub, and did a write-up about difference in developer experience between the two services.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/collaborative-pixelart-drawing.gif" alt="Collaborative Pixelart Drawing">

- [Blog post](https://ably.com/blog/cloud-pubsub-services-compared-azure-web-pubsub-ably)
- [Video](https://www.youtube.com/watch?v=sPgHwm3-yiM)
- [Live demo](https://pixel-paint.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/collaborative-pixel-drawing)

<iframe width="560" height="315" src="https://www.youtube.com/embed/sPgHwm3-yiM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Serverless Pizza Workflow Visualizer

A demo that visualizes the real-time progress of a serverless workflow that is built with Durable Functions. I really had a lot of fun creating the pizza-themed visuals for this demo.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/pizza-workflow.gif" alt="Serverless Pizza Workflow Visualizer">

- [Blog post](https://ably.com/blog/visualize-azure-serverless-workflow-progress-in-realtime-with-pubsub)
- [Video](https://www.youtube.com/watch?v=y9-a_ewgWCQ)
- [Live demo](https://pizza.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/serverless-workflow-visualizer)

<iframe width="560" height="315" src="https://www.youtube.com/embed/y9-a_ewgWCQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Developer tooling

I **really** like improving the developer experience from a tooling perspective. Ably has a Control API, a RESTful interface that allows management of the Ably apps. I've used this API in several tools to speed up the creation of apps and keys.

### Ably Control API GitHub Action

Since most of my demos require a new Ably App, I created a GitHub action that creates an app from a GitHub workflow. The action also enables the the creation of an API key with a configurable set of capabilities. This was the first time I've created a GitHub action, which was a great learning experience for me. The action has a limited feature set, but I intend to add more features in the future.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/ably-control-api-action.png" alt="List Control API GitHub Action">

- [Blog post](https://ably.com/blog/infrastructure-as-code-ably-control-api-github-action)
- [Video](https://www.youtube.com/watch?v=b7GE39JaM3M)
- [Marketplace](https://github.com/marketplace/actions/ably-control-api)
- [GitHub repo](https://github.com/ably-labs/ably-control-api-action)

<iframe width="560" height="315" src="https://www.youtube.com/embed/b7GE39JaM3M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Ably VSCode extension

During the first Ably Innovation Days, I proposed to create a VSCode extension that allows developers to manage their Ably apps directly from VSCode. I formed a team with several colleagues, and in two days we had a working prototype that lists all the Ably apps in the activity bar, and creates a new Ably app via the command palette. We won the *Ship It 🚢* award with this prototype, which allowed me to continue working on the extension and release it to the VSCode extension marketplace.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/vscode-ably-applist.gif" alt="List Ably Apps">

- [Blog post](https://ably.com/blog/announcing-the-ably-vs-code-extension)
- [Video](https://www.youtube.com/watch?v=3CPHb_kn1-o)
- [Marketplace](https://marketplace.visualstudio.com/items?itemName=ably-labs.vscode-ably)
- [GitHub repo](https://github.com/ably-labs/vscode-ably)

<iframe width="560" height="315" src="https://www.youtube.com/embed/3CPHb_kn1-o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Ably CLI

During the second Ably Innovation Days, I started working on specifications for an Ably CLI. After the first day [Phil](https://twitter.com/leggetter) and I started with a prototype based on [oclif](https://oclif.io/). We managed to create a working prototype in a day that lists Ably apps, and creates a new Ably app. This project is still Work In Progress. Once the CLI is in a releasable state, I'll create some content around this.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/ably-cli.png" alt="Ably CLI: List Ably Apps">

- [GitHub repo](https://github.com/ably-labs/ably-cli)

## Events

I had the opportunity to speak at several conferences and meetups this year, both online and in-person. It was great to attend in-person events again, networking with other people, seeing people respond to your talk, and people asking questions afterwards. I'm still a proponent of online (or hybrid) events as well though. Online events can be more accessible, and allow for a more diverse audience.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/dotnetdaysro_speaking.jpg" alt="Speaking at dotnetdays Romania">

Events I spoke at:

- [Azure Lowlands](https://azurelowlands.com/)
- [NDC Oslo](https://ndcoslo.com/)
- [dotnetdays Romania](https://dotnetdays.ro/)
- [CosmosDB Conference](https://www.youtube.com/watch?v=LpzJTJvH6go&t=3453s)
- [ServerlessDays NYC](https://youtu.be/Y0YTtgn5KKo)
- [Microsoft Reactor Toronto](https://www.youtube.com/watch?v=FGklbFQrd44)
- [Oxford .NET User Group](https://www.dotnetoxford.com/posts/2022-02-lightning-talks)
- [Welsh Azure User Group](https://www.youtube.com/watch?v=R87-35-Aiw8)
- [ESPC Community Webinar](https://www.sharepointeurope.com/webinars/start-building-serverless-applications-on-azure/)

## Roadmap & Predictability

Part of my role involves creating and updating a roadmap for the .NET & Azure community. This document refers to the company strategy, and how the DevRel team activities fit in that strategy. The roadmap contains the topics which I plan to create content for, estimations on their outcomes, and a timeline. The metrics & predictability of the outcomes is really one of the most difficult parts in DevRel. One blog post can go viral while the next one hardly gets any engagement. It's important to try understand why something works well or why it doesn't.

What I've learned over the last year is that content creation is just a small part of the bigger picture when working in DevRel. Content distribution is just as important. If I write a great blog post but hardly anyone will read it, then writing it has been a waste of my time. I'm spending more time these days on the distribution part, to make sure the content gets the proper exposure across various channels. It's not a part I particularly like, but it needs to be done, and I do appreciate the new insights it gives me.

Reflecting on the content, the engagement, and analyzing the performance (visits, session duration, sign-ups etc) is even more important. If a content piece is not resulting in (enough) sign-ups, then we investigate why, change the content, or use a different approach for future pieces. We're measuring several aspects for a couple of months now and it still feels like we're at the beginning of understanding the numbers. It's certainly an area that interests me, and where I want to improve the predictability of my work. Being in DevRel means that you're constantly learning, measuring, and tweaking. And that's what I love about it.

## Communication & Relationships

So far, it looks like this is all a one-person-show, but it's far from that.

I depend on:

- My DevRel and content team colleagues, who help me review the content I create.
- Our community manager, who informs me of CfPs, sponsoring opportunities, and new questions on Discord and social media.
- Our content marketing team, to provide me with insights on keyword research and content performance.
- Our product team, to keep me up to date with the latest product features and changes.
- My managers, to provide me with valuable feedback about my work, so I can keep improving.

The *relations* part of DevRel is really crucial. In order to have a good relation with your developer audience, you also need to have a good relation with your team members, Marketing, Product, Documentation, Engineering, and Customer Support. Establishing and growing these relations take time. New people join the company and others leave, it's an ever changing environment. It can be tiring and challenging at times, but once the relationship is there, it feels good to work together to achieve a common goal.

## Anything that could be improved?

Always! 😅 I noticed that I spent less time on my open source projects than before I joined Ably. It's probably because I now create content for a living, I don't always feel like doing that in my spare time as well. I still have plenty of plans to continue working on these projects though, it just needs a bit more planning and prioritization.

One of the areas I really want to expand on is collaborations with other people. These could be collaborations with other DevRel folks, or individual developers who share a common interest around serverless, real-time communication, pixelart, or something completely out of the box! If you think we should combine forces to work on a fun project together, please [reach out](https://twitter.com/marcduiker)!
