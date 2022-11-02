---
layout: post
title: One year at Ably as Sr Developer Advocate
tags: 
---

It has been one year since I've joined the DevRel team at Ably as Sr Developer Advocate. In this post I'll highlight the things I've been working and what I've learned in the past year.

<!--more-->

## Content creation

Before I joined Ably, I mostly worked on event-driven architectures using .NET-based backends and Azure services (Azure Functions, CosmosDB, Service Bus). Ably really shines when real-time communication is needed between clients, or between servers and clients, so this meant I had to get up to speed with client-side programming, and WebSocket based communication using the Ably SDK.

During my onboarding, I created my first demo, [Agile Flush](https://agileflush.ably.dev/), a small and playful web app to do remote planning poker. This was my first introduction to VueJS and the Ably client SDK, and after a while I got to like working with Vue. It did took me some time to get used to the toolchain though.

I've stuck to using Vue (TypeScript based, with Vite as the dev tooling) for most of my demos where I need a front-end. It pairs nicely with Azure Static Web Apps that I use for hosting the demos.

Here's a list with all the demos I've created so far, incl related content pieces.

### Agile Flush

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/agileflush_screenshot.png" alt="Agile Flush web app">

- [Blog post](https://ably.com/blog/tutorial-vuejs-nodejs-azure-static-web-apps)
- [Video](https://www.youtube.com/watch?v=59BZCQuRRkM)
- [Live demo](https://agileflush.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/agile-flush-vue-app)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=59BZCQuRRkM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### What is pub/sub and how to apply it in C# .NET to build a chat app

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/console_chat.png" alt="Console Chat">

- [Blog post](https://ably.com/blog/use-pub-sub-to-build-a-chat-app-with-csharp-net)
- [GitHub repo](https://github.com/ably-labs/pubsub-demo-dotnet)

### Serverless WebSockets Quest

A turn-based mini game with with real-time aspects build on Azure Functions, Durable Functions, and Ably.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/serverless-websockets-play.png" alt="Serverless WebSockets Quest">

- [Blog post](https://ably.com/blog/quest-for-serverless-websockets-azure-functions-adventure)
- [Video](https://www.youtube.com/watch?v=KHzdc3USFU4)
- [Live demo](https://quest.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/serverless-websockets-quest)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=59BZCQuRRkM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Collaborative Pixelart Drawing

A collaborative pixelart drawing canvas. Multiple users draw on a canvas, and their movement and actions are shared with the other connected users. 

I created two versions of this demo, one using Ably, and one using Azure Web PubSub, and did a write-up about difference in developer experience between the two services.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/collaborative-pixelart-drawing.png" alt="Collaborative Pixelart Drawing">

- [Blog post](https://ably.com/blog/cloud-pubsub-services-compared-azure-web-pubsub-ably)
- [Video](https://www.youtube.com/watch?v=sPgHwm3-yiM)
- [Live demo](https://pixel-paint.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/collaborative-pixel-drawing)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=sPgHwm3-yiM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Serverless Pizza Workflow Visualizer

A demo that visualizes the real-time progress of a serverless workflow that is built with Durable Functions. I really had a lot of fun creating the pizza-themed visuals for this demo.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/pizza-workflow.gif" alt="Serverless Pizza Workflow Visualizer">

- [Blog post](https://ably.com/blog/visualize-azure-serverless-workflow-progress-in-realtime-with-pubsub)
- [Video](https://www.youtube.com/watch?v=y9-a_ewgWCQ)
- [Live demo](https://pizza.ably.dev/)
- [GitHub repo](https://github.com/ably-labs/serverless-workflow-visualizer)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=y9-a_ewgWCQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Developer tooling

I **really** like improving the developer experience from a tooling perspective. Ably has a Control API, a RESTful interface that allows management of the Ably apps. I've used this API in several tools to speed up the creation of apps and keys.

### Ably Control API GitHub Action

Since most of my demos require a new Ably App, I created a GitHub action that creates an app from a GitHub workflow. The action also enables the the creation of an API key with a configurable set of capabilities. This was the first time I've created a GitHub action, which was a great learning experience for me. The action has a limited feature set, but I intend to add more features in the future.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/ably-control-api-action.png" alt="List Control API GitHub Action">

- [Blog post](https://ably.com/blog/infrastructure-as-code-ably-control-api-github-action)
- [Video](https://www.youtube.com/watch?v=b7GE39JaM3M)
- [Marketplace](https://github.com/marketplace/actions/ably-control-api)
- [GitHub repo](https://github.com/ably-labs/ably-control-api-action)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=b7GE39JaM3M" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Ably VSCode extension

During the first Ably Innovation Days, I proposed to create a VSCode extension that allows developers to manage their Ably apps directly from VSCode. I formed a team with several colleagues, and in two days we had a working prototype that lists all the Ably apps in the activity bar, and creates a new Ably app via the command palette. We won the *Ship It 🚢* award with this prototype, which allowed me to continue working on the extension and release it to the VSCode extension marketplace.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/vscode-ably-applist.gif" alt="List Ably Apps">

- [Blog post](https://ably.com/blog/announcing-the-ably-vs-code-extension)
- [Video](https://www.youtube.com/watch?v=3CPHb_kn1-o)
- [Marketplace](https://marketplace.visualstudio.com/items?itemName=ably-labs.vscode-ably)
- [GitHub repo](https://github.com/ably-labs/vscode-ably)

<iframe width="560" height="315" src="https://www.youtube.com/watch?v=3CPHb_kn1-o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Ably CLI

During the second Ably Innovation Days, I started working on specifications for an Ably CLI. After the first day [Phil](https://twitter.com/leggetter) and I started with a prototype based on [oclif](https://oclif.io/). We managed to create a working prototype in a day that lists Ably apps, and creates a new Ably app. This project is still Work In Progress. Once the CLI is in a releasable state, I'll create some content around this.

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/02/ably-cli.gif" alt="Ably CLI: List Ably Apps">

- [GitHub repo](https://github.com/ably-labs/ably-cli)

## Events



## Roadmap & Metrics

