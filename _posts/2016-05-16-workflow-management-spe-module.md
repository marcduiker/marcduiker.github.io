---
layout: post
title: "Workflow Management SPE Module"
tags: sitecore PowerShell Extensions Workflow Management
---


<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management.png" alt="Workflow Management SPE Module">


## Sitecore PowerShell Extensions first encounter

Although I had heard of [Sitecore PowerShell Extensions (SPE)](https://marketplace.sitecore.net/Modules/S/Sitecore_PowerShell_console.aspx) several years ago I never used it myself until earlier this month. 
My interest was raised again during the _Accelerated development with Sitecore PowerShell Extensions_ session by [Adam Najmanowicz](https://twitter.com/adamnaj) during SUGCON Europe 2016. 

Right after I got back from SUGCON the project I was working on had a nice challenge. Workflows were being set on existing templates and hundreds of content items had already been created using these templates and had no workflow or workflow state set on them.
We needed a way to update the content items with the workflow that was set on the _Default workflow_ field on the updated templates.

For me this was an excellent opportunity to try SPE. I wrote a Sitecore PowerShell script to update _Workflow_ and _Workflow state_ fields, posted it [in a gist](https://gist.github.com/marcduiker/950e0358bb4752ed5b047931a8c958c1) and [tweeted](https://twitter.com/marcduiker/status/728375187431936000) about it. I got in contact with [Michael West](https://twitter.com/michaelwest101) and Adam Najmanowicz (the developers of SPE) who made some excellent suggestions to improve the user friendliness of the script.

<!--more-->

## Workflow Management Module
Adam asked if I could turn the script into a custom SPE module that could be installed alongside SPE. I really liked this idea and this made me delve deeper in SPE which is btw [extremely well documented](https://sitecorepowershell.gitbooks.io/sitecore-powershell-extensions/content/).

And now the Workflow Management module is ready. Currently only containing one toolbox action: _Update workflow and state for content items_ but more will follow.

### Installation

You'll need SPE v4 and Sitecore 8.x to make use of the module. The zip package can be found in the [GitHub repo](https://github.com/marcduiker/SPE-Modules/blob/master/sitecore-packages/Workflow%20Management%20SPE%20Module-1.0.zip) or on the Sitecore Marketplace (soon).

Install the zip package using the Installation Wizard:

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_install1.png" alt="Workflow Management SPE Module">

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_install2.png" alt="Workflow Management SPE Module">

### Workflow Management toolbox

Once the package is installed you'll see that a _Workflow Management_ element is added to the PowerShell toolbox:

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_toolbox.png" alt="Workflow Management SPE Module">

When you click the _Update workflow and state for content items_ action you'll be presented with the following dialog: 

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_dialog1.png" alt="Workflow Management SPE Module">

Here you need to select the __workflow state__ which is expected to be set on the content items in a later stage. 

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_dialog2.png" alt="Workflow Management SPE Module">

Once you click _Proceed_ the script will do the following:

- Find the workflow for the selected workflow state.
- Find the templates which have the workflow set on the _Default workflow_ field in their ___Standard Values_.
- Find the content items based on these templates and only list those items which _Workflow_ field are empty.

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_report1.png" alt="Workflow Management SPE Module">

To update the content items with the workflow and workflow state you can choose between two actions in the menu:

- Update workflow for all items.
- Update workflow for selected items.

After running one of these actions you will see a notification about the number of items that have been processed.
The list will be updated and now includes the _workflow ID_ and _workflow state ID_ values for the items. 

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/05/16/workflow_management_report2.png" alt="Workflow Management SPE Module">

## Final Thoughts

This was mostly a fun learning exercise for me getting to know Sitecore PowerShell Extensions. I really like SPE and I plan to use it much more for automating tedious manual tasks.

I do hope this Workflow Management module can be of use to others. Please make sure you try it on a development or test environment first before using it on production.

When I tested it on 101 content items it took about 10 seconds to process them on my (outdated) local machine. So be careful when you want to process large amounts of items. 
In that case you might be better of with the [simple script](https://gist.github.com/marcduiker/950e0358bb4752ed5b047931a8c958c1) without the UI. 

If you have any feature requests or issues, please post them on [GitHub](https://github.com/marcduiker/SPE-Modules).