---
layout: post
title: "Hands-on with Sitecore Helix: Setting up automated build and packaging for continuous delivery"
tags: sitecore helix habitat automated build bamboo nuget octopus deploy modular architecture
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/01/03/helix-logical-architecture.png" alt="Modular architecture">

In my [previous]({{ site.url }}/2016/12/28/hands-on-with-sitecore-helix-using-powershell-add-module.html) [posts]({{ site.url }}/2016/12/29/hands-on-with-sitecore-helix-anatomy-add-helix-powershell-script.html) I've shown how our team is able to add new feature or foundation modules easily.
Now let's take a look at the effect of the modular architecture on the build and packaging of a Sitecore Helix style solution.
This is a topic which is receiving loads of attention lately and has been blogged about before by other Sitecore [community](https://www.akshaysura.com/2016/12/27/finally-with-one-great-big-gulp-i-conquered-sitecore-helix/){:target="_blank"} [fanboys](https://www.akshaysura.com/2016/12/28/helix-and-the-re-tooling-of-your-continuous-integration-and-deployments/){:target="_blank"} ;).

<!--more-->

First I need to make clear that the solution I will describe is just one of many approaches that can be taken. 
It is meant to provide insights in the possibilities of building and packaging your Sitecore Helix solution. 
I will share my concerns of this solution in my next post.  

## Modules, Modules Everywhere!

Carefully read the following paragraph from the [Helix Architecure Principles](http://helix.sitecore.net/principles/architecture-principles/modules.html){:target="_blank"}:

> "Keep in mind that in Helix, modules are business-centric. This means that they should relate to business objectives and group together multiple technology entities that refer to this objective. 
> This principle goes against many traditional software conventions - such as the ones dictated by MVC (models, controllers and views) or even Sitecore (templates, layouts, settings) - that define grouping based on their type, rather than their business objective."

The Sitecore Habitat solution currently consists of:

- 16 feature modules
- 14 foundation modules
- 2 project modules

The Visual Studio solution has 54 projects (incl test projects) in total. 
The real world Sitecore project I'm working on at the moment has even more. 
It is definitely a mind shift to work with large numbers of projects. 
Be cautious not create a module for each user story on your backlog though. 
You can of course group or consolidate components which are very similar into one feature. 

### Performance vs Clarity

[Some people](http://sitecore.stackexchange.com/questions/3623/sitecore-helix-habitat-and-visual-studio-structure){:target="_blank"} are against this high number of projects in one Visual Studio solution. 
Although a large number of projects does have a negative impact on Visual Studio performance I encourage the usage of this pattern.
This is because the clarity of this modular architecture outweighs the performance issue. 
Identifying modules is so straightforward now and this greatly helps the communication about these feature and foundation modules. 

As a developer you really need some decent hardware to make this work efficiently in Visual Studio.
If your development machine is slow, do [some calculations](https://docs.google.com/spreadsheets/d/16tzObRLEdgszbxU-un4lG6K-shiE5c39K95aSfrlXvI/edit?usp=sharing){:target="_blank"} so you can make a business case which supports your request to get a new machine.   

So what do you need to do when you have a whole team producing loads of modules? Well, build & package those modules in order to deploy to other 
environments of course so testers and end-users can marvel at your work. 

## Continuous Integration & Delivery

You are all using source control and a centralized build environment, right?

Currently I'm using [GitLab](https://about.gitlab.com/){:target="_blank"} for source control and [Bamboo](https://www.atlassian.com/software/bamboo){:target="_blank"} as the build environment. 
Although Bamboo is not one of my preferred build platforms it does the job well enough. 
In addition [Octopus Deploy](https://octopus.com/){:target="_blank"} is used as the automated deployment environment (more of that in the next post). 

Let's have a closer look at how Visual Studio projects are configured.

### Visual Studio Project Configuration 

By default Visual Studio gives you two solution configurations: `Debug` and `Release`. 
Depending on how you have your environment specific configuration setup you might have more configurations but our team only using these two now.

The `Debug` configuration is the default on during local development. The `Release` configuration is used for the centralized build (more on that later).

I noticed that even in `Release` mode *.pdb (symbol) files are created during a build. 
If you want your release build to have a small footprint (and without debugging capabilities) you can disable to creation of these files by setting the _DebugInfo_ to `none` under _Build_ > _Adanced..._:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/01/03/advanced-build-settings.gif" alt="Set DebugInfo to None">

#### Packaging with Octopack

Octopus provides a __very__ easy way of packaging a solution or projects as NuGet packages through the use of [Octopack](http://docs.octopusdeploy.com/display/OD/Using+OctoPack){:target="_blank"}.
In our solution each Visual Studio project has a reference to Octopack. 
This means that instead of one solution package there are be many packages (one per module). 
This approach does has some drawbacks in the deployment process which I'm not too happy about as I will explain in the next post.

#### Nuspec

A nuspec file contains package metadata and specifies the folders & files will be included in a NuGet package. Octopack can use these nuspec files when NuGet packages are created.  

However you don't __need__ to provide a nuspec file if you're happy with what Visual Studio includes during a build (check the _Build Action_ in the _Properties_ pane if a file is included or not).

But if you want files included which are __not__ included in Visual Studio projects, such as the serialized Sitecore `yml` files in our case, you need to explicitly add that to the nuspec file.
See line 15 in this nuspec template gist:

{% gist cd5c61ebc1d2f303ef3ce456752a8766 %}

Three things might not be too obvious in this nuspec and are worth highlighting:

1. You can move out of the project/solution directory by using the relative path notation `..`.  
2. You need to type `**` in order to include subfolder content.
3. I only added __one__ `<file>` line for the yml files. The standard build output from Visual Studio will also be packaged up but that is configured through a Octopack specific build parameter (`OctoPackEnforceAddingFiles=true`) which I'll describe in the Build section in a moment.

Everything is setup now in Visual Studio. Let's move on to the build server.

### Bamboo

The build plans in are Bamboo are quite straightforward. A plan consists of one stage having one job which has the following three tasks:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/01/03/bamboo-tasks.png" alt="Bamboo tasks">

Currently there are two build plans:

1. One which checks out the `develop` branch and does a build in `Debug` configuration. This plan runs every 15 mins and when does a build when changes are detected in the repository.
2. One which checks out the `master` branch and performs a build in `Release` configuration. This plan is triggered manually. During the build it will create NuGet packages and pushes them to an internal Octopus Deploy NuGet feed. 

I just want to focus on building the solution in `Release` configuration since that part is different since we use the Helix approach nowadays.

#### Build, Package & Publish

The MSBuild task looks as follows:

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2017/01/03/ms-build-task.png" alt="MSBuild task">

The _Options_ field is the most interesting (I've replaced some sensitive info): 

`/p:Configuration=Release;RunOctoPack=true;OctoPackEnforceAddingFiles=true;OctoPackPackageVersion=1.0.${bamboo.buildNumber};OctoPackPublishPackageToHttp=<URL_TO_OCTOPUS_NUGET>;OctoPackPublishApiKey=<OCTOPUS_API_KEY>`

Let's have a detailed look at the build parameters:

- `Configuration=Release` is obvious I hope.
- `RunOctoPack=true` means that Octopack will be run for each of the projects where it has been added as a NuGet reference.
- `OctoPackEnforceAddingFiles=true` ensures that the NuGet package contains both the build output files __and__ the files specified in the nuspec. 
- `OctoPackPackageVersion=1.0.${bamboo.buildNumber}` sets the version number of the NuGet package based on the Bamboo build number.
- `OctoPackPublishPackageToHttp=<URL_TO_OCTOPUS_NUGET>` defines the URL of the Octopus NuGet feed where created packages will be published to.
- `OctoPackPublishApiKey=<OCTOPUS_API_KEY>` contains the API key which is required when publishing to the Octopus NuGet feed. 

Now we're all set to run a build which will publish the NuGet packages for all the modules to Octopus Deploy. 
In the next blogpost I'll go into detail how the deployment process is setup in Octopus.

As already mentioned by my Sitecore community friend [Akshay](https://twitter.com/akshaysura13){:target="_blank"}:

> "I encourage you to discuss Helix/Habitat based conversations in the [Sitecore Slack Helix-Habitat channel](https://sitecorechat.slack.com){:target="_blank"}."