---
layout: post
title: Visual Studio Snippets for Unit Test Methods
tags: unit testing, code quality, code snippet
canonical: "http://blog.marcduiker.nl/2013/04/visual-studio-snippets-for-unit-test.html"
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2013/04/11/NUnit test snippet.png" alt="NUnit test snippet">

##Unit Test Method Naming
As mentioned in a previous post, having a clear naming convention for unit test methods is important to keep tests readable and maintainable. Sticking to the convention might be difficult at first and require some discipline but is definitely worth it in the long run.

###Visual Studio Snippets
In order to make it easy for myself (and other developers) to use the `<UnitOfWork> _ <StateUnderTest> _ <ExpectedBehaviour>` naming convention, I've created two code snippets, one for NUnit and one for MSTest.
After importing the snippets (see below) you can use the them by typing:

`nutest[TAB]` (for NUnit) 

or 

`mstest[TAB]` (for MSTest)

The resulting code is displayed at the top of this post. The snippets use three placeholders (marked in yellow) which make up the method name using the suggested convention. In addition I've added Arrange, Act, Assert comments in order to help structuring the test.

###Importing Snippets
Once you've downloaded the snippet(s) open Visual Studio and go to _Tools > Code Snippets Manager_.

![]({{ site.url }}/assets/2013/04/11/1 Code Snippets Manager.png)

(If you open the Test folder you see two test related snippets already exist. These are `testc` for an MSTest class and `testm` for an MSTest method. The snippets do not use the suggested naming convention though.)

Click the _Import..._ button, select the downloaded snippets and click _Open_. Then select the location to store the snippets (the Test folder seems obvious ;).

![]({{ site.url }}/assets/2013/04/11/2 Import Code Snippet.png)

Click _Finish_ and the snippets are ready to be used. To verify that they are installed or to find out which shortcut they use open the _Code Snippets Manager_ and select the snippet.

![]({{ site.url }}/assets/2013/04/11/3 Check Code Snippet.png)

Reducing some boiler plate coding is always a good thing and I hope you'll find these snippets convenient.

##Resources
- [NUnit code snippet](https://www.dropbox.com/s/86kpsnagd7ftgtc/nunit_testmethod.snippet)
- [MSTest code snippet](https://www.dropbox.com/s/870fi15c4oik5qo/ms_testmethod.snippet)

P.S. Feel free to edit the snippets in order to change the shortcut or alter the code. They're just XML files and there is some [MSDN documentation](http://msdn.microsoft.com/en-us/library/ms165394.aspx) you can work with.