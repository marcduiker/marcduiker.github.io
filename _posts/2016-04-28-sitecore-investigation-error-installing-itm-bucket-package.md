---
layout: post
title: "Sitecore investigation: Errors installing a content package with item buckets"
tags: Sitecore bucket index error
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/04/28/bucket_icon.png" alt="Bucket icon">

A colleague showed me this error today in the Sitecore log:

`ERROR There is no appropriate index for [item path - {GUID}]. You have to add an index crawler that will cover this item`

_'I've never seen that!'_ was my first reaction...

This issue occurred when a Sitecore zip package with content items was installed on another environment. There were quite some lines in the Sitecore log which mentioned the same error but for different item paths and GUIDS. I looked up the GUIDS on my local machine and they were all `Bucket` folder items with no content items in them.

Here's an example of how that looks like (when you've checked the _Buckets_ option in the _View_ toolbar):

<img class="u-max-full-width" src="{{ site.url }}/assets/2016/04/28/item_bucket_without_content.png" alt="Item bucket with an empty bucket folder">

<!--more-->

### Cause

Apparently when _bucketable_ items are deleted their automatically generated parent folders are not deleted. There is a built-in task that periodically removes the empty `Bucket` items but this is disabled by default in the configuration as Raúl Jiménez describes in [this post](http://blog.rauljimenez.co.uk/the-depths-of-the-bucket/).

### Solution

When you use item buckets, make sure you uncomment the `RemoveEmptyBucketFolders` agent section in the Sitecore.Buckets.config as shown below (or even better: make a patch file to enable it):

{% gist 0f1e7c266f5e45dbce269bef841d50ef %}

Finally, always double check if there are empty `Bucket` folder items before making a content package that will contain bucketable items. 

__Case closed__
