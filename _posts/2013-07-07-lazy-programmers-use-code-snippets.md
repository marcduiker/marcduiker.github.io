---
layout: post
title: Lazy Programmers User Code Snippets
tags: .Net, C#, productivity, snippets, Visual Studio
canonical: "http://blog.marcduiker.nl/2013/07/lazy-programmers-use-code-snippets.html"
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2013/07/07/propfull_snippet.png" alt="Full property code snippet">

##The best programmers are lazy

The great [Larry Wall](http://en.wikipedia.org/wiki/Larry_Wall) (author of Perl) claimed that laziness is one of the greatest virtues a programmer could develop. The best programmers are lazy in the sense that they do not write duplicate code or take pleasure in writing boilerplate code. They are efficient in writing code that follows the [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself), [KISS](http://en.wikipedia.org/wiki/KISS_principle) and [YAGNI](http://en.wikipedia.org/wiki/You_aren't_gonna_need_it) principles.

###Efficient IDE usage

Next to writing as few lines of code as possible (without compromising on readability ofcourse), being a lazy programmer also means using the IDE efficiently.

This means:

1. Knowing when and how to use the various project and project item templates.
2. Using keyboard shortcuts instead of navigating through long menus.
3. Using code snippets instead of writing boilerplate code.

###Code Snippets

Ever since the 2005 edition, Visual Studio allows inserting blocks of reusable code, called code snippets, through shortcut keys (or the context menu, but truly lazy programmers don't take away their hands off the keyboard).

I'm still amazed by the fact that so many .Net developers are still not aware of these code snippets. Using these can really speed up writing commonly used bits of code like properties with backing fields, try/catch blocks, foreach loops etc.

__So if you want to be a lazy programmer learn these snippet shortcuts by heart!__

1. `ctor` - constructor
2. `prop` - auto implemented property
3. `propg` - auto implemented property with private setter
4. `propfull` - property with backing field
5. `if` - if block
6. `else` - else block
7. `try` - try/catch block
8. `tryf` - try/finally block (oddly enough there is no trycf!)
9. `for` - for loop
10. `forr` - reverse for loop
11. `foreach` - foreach loop
12. `switch` - switch block

Simply invoke the snippet by typing the shortcut followed by `[TAB]`. [Here's a list](http://msdn.microsoft.com/en-us/library/z41h7fat.aspx) with more built-in shortcuts if you're eager for more.

###Get lazier: write your own snippets!

If you want to be as lazy as me you can easily [write your own snippets](http://msdn.microsoft.com/en-us/library/ms165394.aspx) as I've done for [extension methods]({{ site.url }}/2013/07/04/visual-studio-snippet-for-extension-methods.html), [unit tests]({{ site.url }}/2013/04/11/visual-studio-snippets-for-unit-test-methods.html) and more. The [Snippet Designer plugin](http://visualstudiogallery.msdn.microsoft.com/B08B0375-139E-41D7-AF9B-FAEE50F68392) can be useful if you don't like editing the raw XML.

Enjoy lazy coding!
