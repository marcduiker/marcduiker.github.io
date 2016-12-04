---
layout: post
title: "Ruling the continuous integration seas with Sitecore.Ship - Part 2: fileupload"
tags: Sitecore Ship continuous integration deployment
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/one-does-not-simply-make-a-proper-http-request.jpg" alt="One does not simply make a proper http request.">

## Deploying Sitecore items with Sitecore.Ship

As I mentioned in the [previous post]({{ site.url }}/2015/10/21/ruling-the-continuous-integration-seas-with-sitecore-ship-part-1.html) 
Sitecore.Ship can be used to install Sitecore `update` or `zip` packages by posting HTTP requests to the Sitecore site.

<!--more-->

### Configuration

I forgot to talk about the configuration in the previous post so let's have a look at that now. 
The Sitecore.Ship configuration is split into two parts:

- The __`ship.config`__ file (located in App_Config\Include) contains the 
patched `IgnoreUrlPrefixes` attribute to include the `/services/` url part which Sitecore.Ship is using.

- The __`web.config`__ is updated with a  `packageInstallation` element. 
The default values of this element are:

  `<packageInstallation enabled="true" allowRemote="false" allowPackageStreaming="false" recordInstallationHistory="false" />`

  The attributes are pretty self explanatory. I'll get to the `recordInstallationHistory` in a later post. 
Just make sure it is `false` otherwise there will be errors about a missing _PackageId_.

### Uploading and installing a package
One of the most useful commands of Sitecore.Ship is `fileupload`. When you issue an HTTP request to `<website>/services/package/install/fileupload` 
you can upload _and_ install a Sitecore package.

The [wiki](https://github.com/kevinobee/Sitecore.Ship/wiki/Package-Install-Upload) describes that you need to provide the path of 
the package as form-data in the request. Lets have look how that is done exactly.

#### Postman
The easiest way to test the commands Sitecore.Ship offers is to use an HTTP/REST client such as [Postman](https://www.getpostman.com/),
which I'm using here.

- Once you've started Postman you'll see this screen: 

  <img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/postman_start.png" alt="Postman startup screen">

- Change the following fields to do a post request to upload and install a package:

  <img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/postman_data.png" alt="Postman fileupload post request">
  
- Note that the value of the _Key_ parameter (`path` in this example) is actually irrelevant, it can be any value.
- Once the value type is set to `File` an _Open file_ dialog can be used to select the file to upload.

- Now press the blue Send button to do the post request. 
If everything went well output shows the Sitecore IDs and path of the items that were in the package and have been installed: 

  <img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/postman_result.png" alt="Postman fileupload response">

So far so good, but we don't want to use Postman manually in order to upload and install packages for every deployment right?
We need a solution that can be automated and used in a continuous integration setup. 

#### cURL
I first looked into PowerShell and the `Invoke-RestMethod` command but it appeared that OOTB this method does not support 
multipart form data, which is required to call the `fileupload` command.
There is [a workaround](http://stackoverflow.com/a/25083745/112544) to create the required multipart boundaries
in the request but I did not like this approach. I looked for another solution and found cURL.

[cURL](http://curl.haxx.se/) is a very powerful commandline application to script HTTP jobs. 
Getting the syntax right can be a little tricky although there is quite some [documentation](http://curl.haxx.se/docs/httpscripting.html).
Luckily Postman can generate various scripts including one for cURL:

<img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/postman_generate.png" alt="Postman generate code">

<img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/31/postman_curl.png" alt="Postman cURL code">

However the cURL script in the screenshot above contains a lot of unncessesary statements and actually gives errors.
I've found that this is the minimal cURL syntax which works for me:

`curl -F "path=@<path to update or zip package>" 'http://<website>/services/package/install/fileupload'`

Sofar we've only replaced Postman with cURL, but since cURL is a commandline tool it can be easily called 
from a script during a deployment process as we'll see next.

#### PowerShell

In the Gist below you can see the _deploy-sitecorepackage.ps1_ script which I use to upload and deploy Sitecore packages.
I actually prefer to use the more verbose cURL syntax (e.g. `--form` instead of `-F`) because I believe the intention
of the script is much more clear to the reader who might not know the syntax well. 
A full description of the parameters can be found in the [cURL manual](http://curl.haxx.se/docs/manpage.html).

{% gist 6e3ada7efad84dc418b7 %}

The _deploy-sitecorepackage.ps1_ script uses another script called _get-curlpath.ps1_ to obtain the path to the cURL executable.

{% gist 345f5aa6e0e55accb81b %}

These PowerShell scripts can now easily be used in continuous integration & delivery tools such as Octopus Deploy or Microsoft Release Management.

### What's next?
In the next post I'll explain how Sitecore.Ship can be used to record the package installation history.