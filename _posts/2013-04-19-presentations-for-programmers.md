---
layout: post
title: Presentations for Programmers
tags: presenting html5 soft skills
canonical: "http://blog.marcduiker.nl/2013/04/presentations-for-programmers.html"
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2013/04/19/Presentations for programmers.png" alt="Presentations for programmers">

## Great Slides + Great Speaker = Great Presentation

In the last few years I find myself doing more and more (technical) presentations. I have to admit I even start to like it. I absolutely admire great speakers like [Scott Hanselman](http://www.hanselman.com/) (Microsoft) and [Tim Ward](https://twitter.com/jerrong) (Sitecore/CluedIn). They speak with such passion about technology that I immediately want to use or do whatever they promote. Although my presentation skills are nowhere near the level of these speakers I do have a good grasp of what makes a great presentation. So before becoming a great speaker lets start with creating great slides.

<!--more-->

## Presentation Tools

Regardless of the content, a tool is required for creating slides and doing presentations. For quite some time I've been on a quest to find a tool that fits all my needs.

### Powerpoint
As many I've started with Microsoft PowerPoint which is a very easy tool to create standard (and therefore boring) slides. I actually find it difficult to create interesting and captivating presentations with PowerPoint and that's why I don't use it.

### Prezi
Next I tried [Prezi](http://prezi.com/) for a while. Initially I was really blown away by this platform with its possibilities to zoom, translate, rotate etc. I still very much like these effects (when used in moderation) but I had some issues with applying custom styles and using the Prezi offline. The major downside for me is the lack of offline editing in all but the most expensive licenses. Another nuisance is the size of the presentations when used offline, these are huge compared to other platforms.

### HTML5
And now there is [HTML5](https://developer.mozilla.org/en/docs/HTML/HTML5). And I love it. A lot of HTML5 presentation frameworks are emerging and currently I'm experimenting with [reveal.js](http://lab.hakim.se/reveal-js/#/) (written by [Hakim El Hattab](http://hakim.se/)). Really have a go at this one if you haven't seen it yet!


[![Reveal.js][4]][3]

[4]: http://lab.hakim.se/reveal-js/#/
[3]: {{ site.url }}/assets/2013/04/19/reveal.png

An HTML5 presentation framework is just a collection of html, css and javascript files which you have complete control over (yay!). You can tweak the markup, styles and functions as desired. That immediately is also the downside for people who don't have any web development skills (but they should stick to PowerPoint anyway ;). You need to know a bit of html to get started (although reveal.js has an online editor called [slides.com](http://slides.com/)). The presentation can be used either online or offline, it all works the same, only an up-to-date browser is required (I prefer Chrome).

## How it's made

I used [Sublime Text 2](http://www.sublimetext.com/2) to create the html (based on Hakim's [template](https://github.com/hakimel/reveal.js/blob/master/index.html)) and a custom css theme (I named it [programmer.css](http://marcduiker.azurewebsites.net/presentations/reveal.js/css/theme/programmer.css)). I used a monospaced webfont called [Inconsolata](http://www.google.com/fonts/specimen/Inconsolata) in order to get an IDE look and feel. The color scheme is based on the Monokai theme. Feel free to re-use or adapt the programmer.css theme in your own reveal.js presentations.