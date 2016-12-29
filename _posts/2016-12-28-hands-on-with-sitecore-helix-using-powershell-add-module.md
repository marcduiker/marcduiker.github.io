---
layout: post
title: "Hands-on with Sitecore Helix: Using PowerShell to add a new module"
tags: powershell sitecore helix habitat dte envdte visual studio
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2016/12/28/powershell_vs_sitecore_helix.png" alt="PowerShell plus Visual Studio equals Sitecore Helix">

## Embracing Sitecore Helix

Ever since I attended [Anders Laub](https://twitter.com/AndersLaub) his presentation at SUGCON Europe 2015 about component based architecture in Sitecore solutions I have been a strong advocate of these modular principles. 
I even went to [Pentia](http://www.pentia.net) to learn about this in full detail. 

I was very happy to see that Sitecore finally got their act together and published their [Helix](http://helix.sitecore.net/) guidelines and recommended practices on the web. 

For the last half year my team is using the modular Helix style architecture with success and it's time to share some experiences.

<!--more-->

__Go straight to the [TL;DR](#tldr)__

## Adding a new module with ease

Seeing a Sitecore Helix solution can be a bit daunting at first. 
The folder structure is quite deeply nested and developers need to have a good understanding what a module is composed of.
Adding a new Feature or Foundation module to your Sitecore Helix solution is a time consuming and error prone task if you do it manually over and over again. 
Just as a learning experience it's good to know what needs to be done so please [do have a look](https://www.youtube.com/watch?v=4lC-SdYh4Xg). 

Since adding a module is such a repetative task it is a perfect candidate for automation. Currently there are two [Yeoman](http://yeoman.io/) based solutions to create modules; [`generator-habitat`](https://github.com/kamsar/generator-habitat) and [`generator-prodigious-helix`](https://github.com/mrodriguezr/generator-prodigious-helix).

### Pros

Both these Yeoman generators allow you to create a new Feature or Foundation module with ease. Both create the folder structure and the Visual Studio projects. 
The main difference is that `generator-habitat` works with [Unicorn](https://github.com/kamsar/Unicorn) and `generator-prodigious-helix` works with [TDS](http://www.teamdevelopmentforsitecore.com/).

### Cons

The only drawback of both generators is that they don't update the solution file. 
So you manually need to add the generated projects into he solution (and create the module specific solution folder as well).

Because I'm [lazy](http://threevirtues.com/) and don't want to do repetitive work I set out to find another solution for this. 

## PowerShell

Since I'm more familiair with PowerShell I used that instead of the Yeoman generators (I already invested quite some time in my own solution before I became aware of the Yeoman generators). 
Fairly quickly I had a script that would copy a template folder to the desired destination and replace tokens for the module name, namespaces and GUIDs. 

The only thing left was adding the projects to the solution. 
First I took a very basic approach and started parsing the sln file since it's all plain text anyway.
But it became quite a hassle to manage project relations and nesting of projects and solution folders with only GUIDs to work with.
I needed a better solution. And then I met DTE.

## DTE to the rescue

[`DTE`](http://stackoverflow.com/questions/17239760/what-is-the-visual-studio-dte) (Development Tools Environment) or `EnvDTE` is the [Visual Studio automation model](https://msdn.microsoft.com/en-us/library/envdte._dte.aspx) and is used for Visual Studio extensions to manipulate the solution and it's projects. The `DTE` framework (COM based) is implemented across several `EnvDTE*.dll` and `VSLangProj*.dll` libraries depending on the version of Visual Studio you're running.

The [`SolutionFolder`](https://msdn.microsoft.com/en-us/library/envdte80.solutionfolder.aspx) interface in the `EnvDTE80` assembly captured my interest with the following methods:

- [`AddSolutionFolder`](https://msdn.microsoft.com/en-us/library/envdte80.solutionfolder.addsolutionfolder.aspx)
- [`AddFromFile`](https://msdn.microsoft.com/en-us/library/envdte80.solutionfolder.addfromfile.aspx)

In my initial attempts of using DTE I experienced quite some difficulties in creating the right types of objects and interfaces. 
This was probably due to not having the correct `EnvDTE*.dll` and `VSLangProj*.dll` assemblies loaded in my PowerShell script.

I found that the NuGet Package Manager Console in Visual Studio already has the proper assemblies loaded since it's also using `DTE` when adding new NuGet packages to the solution.
Now I only needed to find a way to call my PowerShell script from the Package Manager Console.  

## NuGet profile

The PowerShell commands that can be used in the Package Manager Console are stored in a `Profile.ps1` script located at `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\<someweirdID>\Modules\NuGet`.
Since that profile belongs to the console it's probably best to not touch that one because it could be overwritten during an update.

When you type `$profile` in the Package Manager Console you'll get the location of a user profile that can be used to extend the default one. 
In my case I got the following:

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/12/28/nuget_profile_path.png" alt="NuGet Profile Path"> 

If you go to that location there might not be a profile file at all. You can then create an empty file and name it `NuGet_profile.ps1`. 

When you make changes to this user profile while Visual Studio is open Visual Studio will not detect any changes. You can type `& $profile` in the Package Manager Console to reload the profile.  

## <a name="tldr"></a> TL;DR: My Add-HelixModule Solution

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/12/28/add-helixmodule.png" alt="Add-HelixModule">

1. `module-template`, a folder containing code and config template files. This template is based on a Sitecore Habitat Feature which is stripped down significantly.
2. `add-helixmodule.ps1`, a PowerShell script which creates a new Feature or Foundation module __AND__ adds this to the current solution in Visual Studio.
3. `NuGet_profile.ps1`, a PowerShell NuGet profile that is used by the Visual Studio Package Manager Console. This profile only loads the add-helixmodule.ps1.
4. `add-helix-module-configuration.json`, a config file containing values for namespaces, location of the module-template and more.

You can see it working in my fork of Sitecore Habitat:

- Clone my [Sitecore Habitat fork](https://github.com/marcduiker/Habitat).
- Verify that you have a `NuGet_profile.ps1` (use `$profile` to check the location).
- Add the following to this profile and update the path to point to the `add-helixmodule.ps1` file on your disk:

{% gist d519bb402dc199d350c05de8bf696231 %}

- Open the Sitecore Habitat solution in Visual Studio.
- Open the `add-helix-module-configuration.json` file.
- Update the following values in that configuration file:
   - `moduleTemplatePath`. This is the absolute path to the module-template folder.
   - `featureNamespacePrefix`. This is the namespace prefix for new Feature modules (e.g. CompanyName.ClientName).
   - `foundationNamespacePrefix`. This is the namespace prefix for new Foundation modules (e.g. CompanyName).
   - `sourceFolderName`. This is the relative path to the location where the Feature/Foundation and Project folders are. For Sitecore Habitat this is `//src`.
  
{% gist 0b7c846430e8c9e89297dbf61ad829f3 %}

- Type `Add-Feature` or `Add-Foundation` in the Package Manager Console followed by the name of the module and hit enter. 

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/12/28/add_feature_completed.png" alt="Add-Feature Completed">

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/12/28/solution_explorer.png" alt="Solution Explorer showing the added feature">

In my [next post]({{ site.url }}/2016/12/29/hands-on-with-sitecore-helix-anatomy-add-helix-powershell-script.html) I'll dig deeper 
into the inner workings of the `add-helixmodule.ps1` PowerShell script.

## So are we done now?

No. Currently the script only works with an existing Visual Studio solution that uses the Helix/Habitat folder structure so it heavily relies on folder names called `Feature` and `Foundation`.

Improvements I can think of now: 

- Make the script more robust/configurable so it works with other naming conventions instead on Feature/Foundation. 
- Add yml files for rendering and template folders similar as `generator-habitat` is doing.
- Extend the script so it could also create a whole new Sitecore Helix based solution.
- Use separate template structures for Feature and Foundation modules. 

A whole different approach will be to investigate the experimental [`vs-net-dte`](https://github.com/elkdanger/vs-net-dte) and [`gulp-notify-dte`](https://github.com/elkdanger/gulp-notify-dte) projects in order to get DTE to work with Gulp. Please do let me know if you have plans to dig into this :).