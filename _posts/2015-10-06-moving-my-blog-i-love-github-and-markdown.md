---
layout: post
title: "Moving my blog - I <3 Github &amp; Markdown"
tags: Github markdown Jekyll
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2015/10/06/successkid-blog.jpg" alt="Moved blog to Github, writing posts in markdown.">

## The blogging experience

Blogging should be hassle free. With my old blog platform, blogging was far from that. 

Party this was due because I deviated from the built-in themes and I hacked something custom into it. I always needed to manipulate the html of a post because the paragraphs tags and white space were off... very annoying. 

And when blogging gets tedious because of such things you blog less and less. It's been almost a year since I posted anything, which is of course unacceptable ;).

<!--more-->

## Moved!
So, it was time for desperate measures and I decided to move my blog to Github.

### Host: Github Pages
Github has a really great feature called [Github Pages](https://pages.github.com/). Github Pages is primarily intended to create static websites for projects you have in Github repositories. It also allows a website per Github user or organisation (`http://<username>.github.io`). For my blog I decided to have a Github Pages [user website](http://marcduiker.github.io).

### Blog engine: Jekyll
The website is generated through [Jekyll](http://jekyllrb.com/). This is a very straightforward static site generator built with [Ruby](https://www.ruby-lang.org/) and has good support for blogging.

#### Templates: Liquid
Don't be fooled with the term 'static site'. It certainly does not mean you have to write a full html page for each post. Jekyll uses the [Liquid templating engine](https://github.com/Shopify/liquid/wiki) and with it you can break down an html page in reusable components (for head, header, footer, content etc).

I used [Visual Studio Code](https://code.visualstudio.com/) to create the html components.

#### Content: Markdown
Now comes the part which I'm most content with (pun intended). Jekyll supports [Markdown](http://daringfireball.net/projects/markdown/) as the format for the blog posts. I __so__ like this format because of it's ease of use and minimalism.

#### Responsive design &amp; style: Skeleton
Since I didn't choose one of the default Github Pages templates the blog looked very 90's with only a plain Times New Roman font. That needed to change but I do like a very minimalistic style.

My front-end skills are quite limited so I looked for a very simple responsive boilerplate framework to work with. I decided to go for [Skeleton](http://getskeleton.com/).

## I <3 blogging again

Now the blog looks good again and writing posts has become much simpler and more fun for me. 

### My blogging workflow

- I have a local (and up-to-date) repository of the remote [`marcduiker.github.io`](https://github.com/marcduiker/marcduiker.github.io) repo.
- I copy the default blog post Markdown file and give it a proper name according to the Jekyll naming convention.
-  I start editing the content of blog post file using [MarkdownPad2](http://markdownpad.com/).
- When I can't finish the post in one go I save it in the `_drafts` folder.
- When I finish the post I save it in the `_posts` folder. All markdown files in this folder will be publicly visible.
- I commit &amp; push the post (and relevant assets) to the remote repository.

__And that's all there is.__

_Note: I only use MarkdownPad2 for editing and previewing the blog post content. Because I'm not running Jekyll on my local machine I can't see a fully rendered page before actually pushing it online. Since it's so quick to make a change and seeing the rendered result online I'm fine with this approach._


### Why do blogging like this
The workflow and tooling involved feel very natural if you're a developer. I also like the idea of having my content in version control.

So if you're a developer who is either not blogging or want to switch to another blogging platform really have a look at [Github Pages](https://pages.github.com/).

Expect some new Sitecore related posts from me soon! :)

