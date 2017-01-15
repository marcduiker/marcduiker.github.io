---
layout: post
title: "Hands-on with Sitecore Helix: Continuous Delivery with Octopus Deploy"
tags: sitecore helix habitat automated nuget octopus deploy modular architecture
---

<img class="u-max-full-width" itemprop="image" src="" alt="">

In my [previous post]({{ site.url}}/2017/01/03/hands-on-with-sitecore-helix-setting-up-automated-build-packaging-continuous-delivery.html) I discussed how to setup an automated build for a Sitecore Helix project. 
In this post I'll be covering the configuration of the automated deployment environment: [Octopus Deploy](https://octopus.com/){:target="_blank"}.

<!--more-->

## Octopus Deploy

Octopus Deploy has [great documentation](http://docs.octopusdeploy.com/display/OD/Getting+started){:target="_blank"} so I won't be going over all the details. 
I will highlight the major components and mention the Sitecore (Helix) specific configuration. 

### Environments

### Lifecycle

### Process

### Variables

### Creating Releases

I've setup the release versioning to use the version from the NuGet Packages:

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/01/15/release-versioning.png" alt="Release versioning">

A release can be created automatically when Octopus detects a new package being pushed to its NuGet feed.
This is configured by selecting a NuGet Package deployment step from the process.  

<img class="u-max-full-width" src="{{ site.url }}/assets/2017/01/15/automatic-release-creation.png" alt="Automatic release creation">

Although this functionality sounds great I would recommend __not__ to use this!

When I tried this automatic release creation I found out that releases often would contain old versions for some packages.
This discrepancy was caused by new packages not being available yet on the Octopus NuGet feed when the release was created.
Octopus triggers the release creation based on __one__ NuGet package while a Helix solution will produce many packages (in this setup). 
So if the package referred to in the NuGet Package deployment step is __not__ the last package to be pushed by the build server a release will be created in 
Octopus which still refers to some old packages.
Since the build order of the projects (and thus the push of the packages to Octopus) might change it is too risky to have the released created automatically 
based on the presence of one package of which you assume is the latest in the build process.  

It is much better to trigger the build by running a command line tool called `Octo.exe`.
This tool needs to be [installed on the build server](http://docs.octopusdeploy.com/display/OD/Bamboo#Bamboo-Creatingarelease){:target="_blank"} and a post build step needs to be added which calls `Octo.exe` with the following arguments:

`create-release --project <PROJECT_NAME> --version 1.0.${bamboo.buildNumber} --packageversion 1.0.${bamboo.buildNumber} --server <URL_OCTOPUS_SERVER> --apiKey <OCTOPUS_API_KEY> --releaseNotes "Some notes here about the release"`

Let's have a look at the argument switches:

- `create-release` is clear right?
- `--project <PROJECT_NAME>` specifies the project name in Octopus Deploy to create a new release for.
- `--version 1.0.${bamboo.buildNumber}` sets the version of the release based on the Bamboo build number.
- `--packageversion 1.0.${bamboo.buildNumber}` defines which version of the NuGet packages will be used in the release.
- `--server <URL_OCTOPUS_SERVER>` specifies the Octopus Deploy server URL.
- `--apiKey <OCTOPUS_API_KEY>` specifies the API key which is required to execute the create release command.
- `--releaseNotes "Some notes here about the release"` Optional switch for the release notes.

An alternative of using the command line tool would be to use the [Octopus REST API](http://docs.octopusdeploy.com/display/OD/Octopus+REST+API){:target="_blank"}.
The [OctopusDeploy-API GitHub repo](https://github.com/OctopusDeploy/OctopusDeploy-Api/tree/master/REST/PowerShell){:target="_blank"} contains loads of PowerShell scripts on how to use the REST API.

## Multiple Nuget packages: The good, the bad and the ugly

As described in my previous post I took the approach of having NuGet packages built for each module in our solution.

The reason behind this approach was twofold:

1. It was the quickest solution since it only required to have the OctoPack NuGet package added to the projects.
2. I liked the idea of having the ability to deploy modules in a granular way.


