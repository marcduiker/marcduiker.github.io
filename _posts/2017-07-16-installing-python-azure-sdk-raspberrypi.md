---
layout: post
title: "Installing the Python Azure SDK on a Raspberry Pi Zero"
tags: azure sdk python raspberry pi zero
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/07/16/Pi Zero with camera.jpg" alt="Pi Zero with camera module">

## Holiday Project

This summer holiday I'm working on a hobby project which involves a Raspberry Pi Zero and a Pi camera module. Part of the solution is uploading the pictures the Pi takes to the cloud, Microsoft Azure to be more specific. I plan to write a couple of blog posts about this project. This first post is about installing the Azure SDK on the Pi Zero.


<!--more-->
### Python and Azure

[Raspbian Jessie](https://www.raspberrypi.org/downloads/raspbian/), the Linux distribution for Raspberry Pi already has Python pre-installed and since Microsoft provides an Azure SDK for Python I decided to write an application in Python which will capture and upload the images to Azure.

### Errors when installing the Azure SDK

When I tried installing the [Azure SDK](https://pypi.python.org/pypi/azure) (v2.0.0) using: 

`sudo pip install azure` 

I got some error messages such as:

- `Expected version spec in', 'azure-batch ~=3.0.0'`
- `fatal error: ffi.h: No such file or directory`
- `fatal error: openssl/opensslv.h: No such file or directory`

Apparently the  Python environment on my Pi Zero was out of date and missing some libraries. These were the steps I took to get to a successful installation of the Azure SDK:

- Update pip: `sudo pip install --upgrade pip`
- Install libffi: `sudo apt-get install libffi-dev`
- Install libssl: `sudo apt-get install libssl-dev`

### Clean up

Since I want to keep my SD card as clean as possible I cleared the cached Python packages located in `/var/cache/apt/archives` by running this command:

`sudo apt-get clean`

In the next blog post I'll show which Azure services I'm using and how I configured them.