---
layout: post
title: "Blog upgrade: Post excerpts, microformats and improving performance"
tags: blog microformat excerpt jekyll cdn performance seo
---

## Time for an upgrade

I finally made some time available to work on this blog. There were a couple of things I wanted to improve since [I moved to GitHub Pages/Jekyll]({{ site.url }}/2015/10/06/moving-my-blog-i-love-github-and-markdown.html).

1. Better looking index page. Before todays change it was a __very__ simple (and boring) bulleted list of links containing the date and title of individual blogposts.
2. Improved content structure. Having a well defined content structure improves SEO. 
3. Higher performance. Faster page loads makes both users and Google happy. 

<!--more-->

## Index page with blog excerpts

I wanted more body in the index page but I didn't want a huge page with full blog posts. 
Luckily Jekyll supports the usage of [post excerpts](https://jekyllrb.com/docs/posts/#post-excerpts) out of the box. You add the excerpt seperator to the __config.yml_: 

`excerpt_separator: <!--more-->`

And in the content of the post where the excerpt ends you place the seperator:

```
## Title

This is included in the excerpt.

<!--more-->

This is not included int the excerpt.

```

Jekyll will use the content before the seperator as the excerpt. This can be used in the index page when iterating the posts as:

```
{% raw %}
{% for post in paginator.posts %}
  ...
  {{ post.excerpt }}
  ...
{% endfor %}
{% endraw %}
```

Super easy. Let's continue with structuring the content.

## BlogPosting microformat

I stumbled on [this page](http://greyfocus.com/2015/05/schema.org-microformat-jekyll/) on how to use a blogpost microformat with Jekyll.

Although I heard of microformats before I never really took the time to look into it. It appears that there are microformats for [nearly everything](https://schema.org/docs/full.html)!
Ranging from the most basic [`Thing`](https://schema.org/Thing) to [`AdultEntertainment`](https://schema.org/AdultEntertainment) to [`Volcano`](https://schema.org/Volcano) (notice the `smokingAllowed` propery on that one ;) ).

The most useful for blog writers is the [`BlogPosting`](https://schema.org/BlogPosting) microformat. I'm currently using the following properties in the `post.html` layout:

- `itemscope itemtype="http://schema.org/BlogPosting"` on the `article` element.
- `itemprop="name"` on the `h1` which contains the blog title.
- `itemprop="datePublished"` on the `time` element.
- `itemprop="auhor"` and `itemscope itemtype="http://schema.org/Person"` on the `span` which contains my name.
- `itemprop="articleBody"` on the `div` that contains the post body content.

Google provides [an online testing tool](https://search.google.com/structured-data/testing-tool) to verify structured website data. 
Currently I'm still missing _publisher_ and _image_ data but search engines should already be able to index my blog posts better which will improve SEO.

## Improving performance

The last thing on my list was to improve the performance of my blog. [YSlow](http://yslow.org/) and [Google PageSpeed](https://developers.google.com/speed/pagespeed/insights/) scores were around 80 out of 100 which is not bad to begin with.
But both of these tools indicated that using a [content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network) (CDN) would improve performance. I've used CDN solutions for work projects but never considered it for personal projects like this blog. 

I found that [CloudFlare](https://www.cloudflare.com/plans/) offers a free plan for personal websites so I signed up right away. I was quite surprised that this free plan still allows a great deal of configuration. A useful feature is the _developer mode_ which disables the caching temporarily so you can see your changes quickly.

The YSlow score for this page is now at 92 and Google PageSpeed is at 85 and I'm quite content with those numbers. Now it's time to write more posts again and get the traffic going! 