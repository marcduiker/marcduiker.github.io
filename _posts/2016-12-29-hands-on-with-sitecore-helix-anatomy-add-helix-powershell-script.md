---
layout: post
title: "Hands-on with Sitecore Helix: Anatomy of the Add-HelixModule.ps1 PowerShell script"
tags: powershell sitecore helix habitat dte envdte visual studio
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2016/12/29/add-feature-script.png" alt="Add-Feature PowerShell function">

In my [previous post]({{ site.url }}/2016/12/28/hands-on-with-sitecore-helix-using-powershell-add-module.html) I 
showed how I got to a solution which allows the developers in my team to create new Feature and Foundation modules with ease.

I showed the moving parts of the solution but I did not go into much detail of the most important part so that's what I'll do in this post. 
This would be particularly useful if you want to change the script yourself to match it to your needs.

<!--more-->

## A detailed look at add-helixmodule.ps1

The `add-helixmodule.ps1` script is where all the action happens. The file is [included in my Habitat fork](https://github.com/marcduiker/Habitat/blob/master/scripts/add-helixmodule.ps1) and is also available  
as a gist which is shown inline below.

I've added loads of comments to it today so I think should give you enough to work with. 
The function which handles the addition of projects to the solution through the [DTE interface](https://msdn.microsoft.com/en-us/library/envdte.dte.aspx) is called `Add-Projects` (how surprising!) and starts at line 283.

Please do let me know if you have comments or suggestions for improvements!    

{% gist 75a5aadffa8e8bec953dc936500a13c0 %}

