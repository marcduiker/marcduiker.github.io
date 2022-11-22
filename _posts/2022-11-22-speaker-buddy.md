---
layout: post
title: Be ready for failure on stage: the speaker buddy system
tags: 
---


<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2022/11/22/.png" alt="Speaker Buddy System">

<!--more-->

## Conferences

I love ❤️ going to conferences, especially when I'm invited to speak there. Although I've been speaking at dozens of conferences and meetings over the past years, I still have impostor syndrome when it comes to speaking. Some people really seem to have a gift for public speaking, and sometimes I feel way out of my league. I do realize those experienced speakers have been practicing for years, and perhaps I'll get there as well, practice makes ~~perfect~~ improvement! 💪


## Failing on stage

One thing that can throw me off during my speaking sessions is technical issues. And I don't mean failing demos (I'm used to those 🫠), but audio/video issues that require minutes (feels like hours 😭) to solve.

A couple of weeks ago, I had such an issue where my laptop didn't mirror the screen to the AV system. I was using a MacBook that I used a dozen times before, but which I'm not very familiar with, and even when I got some help (and multiple adapters), the issue wasn't solved. It was a few minutes past the official start time of the talk, and I was now getting nervous (and I'm quite a chill person otherwise). Luckily the previous speaker ([Mattias Karlsson]()) had a brilliant idea: he asked me to install TeamViewer on my laptop, connect to TeamViewer session on his laptop, and he would connect his machine to the AV system. This worked like a charm (WiFi was reasonable), and I was able to start my talk about 5 minutes late. 🎉 The stress I built up during those minutes was not beneficial for my delivery though. My breathing was shallow, and I my pace was too fast.

Could I have prevented the issue? No, but I could have been more prepared.

## Preparation

There are several layers of preparing for technical issues. I'll share some of my ideas and an idea that Mattias came up with at the end of the conference.

I won't cover the 'demo doesn't work' part here since that is already described in other places.

The general idea is to try to ensure that your presentation and demos are available on other environments you can quickly access.

Let's go through some solutions:

### Bring a second laptop

Yes, it can be that easy. But one, you need to *have* a second laptop (💸💸💸), and two, you need to take it with you when traveling, which isn't always an option. I have a small Windows Surface Go that usually bring with me next to my main laptop.

### Cloud storage

For things that doesn't require showing or running code, you can use a cloud storage solution like OneDrive, Google Drive, or Dropbox. I use OneDrive to store and access my presentations. If your laptop fails, you still need to use another machine, but you can access your presentation relatively quickly.

### Cloud hosted coding environments

When you do need to show, or run code in your session make sure you can run it in the cloud instead of your own machine! I have all my demos on GitHub, and I'm using [GitHub Codespaces](https://github.com/features/codespaces). But there are many other solutions out there; [Gitpod](https://www.gitpod.io/), [JetBrains Space](https://www.jetbrains.com/space/), [CodeSandbox](https://codesandbox.io/).
The downside of this solution is that it does require a good internet connection.

> **TIP**: If your source code is on GitHub, and you just need to show source code, you can use [github.dev](https://github.com/github/dev), a lightweight, browser-based editor (based on VSCode) you can access by replacing `.com` with `.dev` when typing the repository URL.

## The speaker buddy system

This is the idea that Mattias came up with, and I love it! I named it the **Speaker Buddy System**, but I'm open to better names. In the above mentioned solutions, you still need access to another laptop, and this can be a solution for that!

### For conference speakers

Once the conference agenda is published, contact another speaker, that does not speak at the same time as you, and ask if they are willing to help you out in case of technical issues. If they are, you can discuss the details such as:

- Have a copy of your presentation on their machine.
- They could clone a code repository, if you want to show code locally.
- They could install some tools you need, if you want to run things locally.

Even with the existence of cloud-based coding environments, I still suggest to have a local backup of this in case the internet connection is not good enough.

### For conference organizers