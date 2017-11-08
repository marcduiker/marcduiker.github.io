---
layout: post
title: "Improving unit test readability: helper methods &amp; named arguments"
tags: unit test readability named arguments C# .Net
---

'Uncle Bob' wrote the following in <a rel="nofollow" href="https://www.amazon.co.uk/gp/product/0132350882/">Clean Code: A Handbook of Agile Software Craftsmanship</a>:

_"The ratio of time spent reading (code) versus writing is well over 10 to 1 ... (therefore) making it easy to read makes it easier to write."_

So think about that when you write your next bit of code. You need to make sure your code is easily readable and understandable for _others_, you hardly ever write code just for yourself.

<!--more-->

## Readable unit tests

Unit test code is not different than 'production' code. Readability is key here because unclear unit tests will be distrusted, ignored and possibly removed.

### Bulky Arrange section

Consider the following unit test for testing the `GetHighestRatedMovies` method in an imaginary `MovieService`:

{% gist 9db3de2f8fb70121e77cd6c6423164b7 %}

Although this unit test uses some great frameworks such as [xUnit](https://xunit.github.io/), [AutoFixture](https://github.com/AutoFixture/AutoFixture), [FakeItEasy](https://github.com/FakeItEasy/FakeItEasy) and [FluentAssertions](http://www.fluentassertions.com/) (big fan of these!) there are still some things I don't like, particularly in the _Arrange_ section:

- In lines 5-6 a collection of `Movie` objects is set up using _AutoFixture_. I get why this collection is neccesary but I really don't care about __how__ it's done. In addition you could argue that 20 is a magic number although the intent is quite clear here. It can definitely be a bit more clear.
- In lines 7-8 a fake object based on `IMovieRepository` is created using _FakeItEasy_. The fake repository is required to be able to return `Movie` objects from the `MovieService` but again I don't really care __how__ that is done.
- In line 9 the `MovieService` is instantiated and the fake repository is passed in the constructor. But what are the `null` arguments there? What do they represent?
- In line 10 a `MovieServiceRequest` object is constructed. It's a simple value object with just one property. But what will happen if more properties are added later? Then the construction of this request will take up quite some space which has a negative impact on readability.  

In general I feel there is too much detail in this _Arrange_ section which is not relevant for understanding the unit test. 
Although 6 lines is not that much I do believe fewer lines in the _Arrange_ and clear usage of arguments will improve the readability a lot.

### Lean Arrange section

Here's how I refactored the unit test:

{% gist d92aa66366c27459bd81cdf162aa0b7a %}

There are two major differences:

- Creation of fake objects and the request object is now done in private methods. This allows re-usage of these methods in future unit tests. When the number of these helper methods grows you should consider moving them to a seperate class.
- [Named arguments](https://msdn.microsoft.com/en-us/library/dd264739.aspx) (available since C# 4.0) are used when calling the helper methods and for constructing the `MovieService`. It is now evident what the `null` values actually represent. An alternative would be to declare seperate variables for all these arguments. Although that would be very clear and descriptive I believe that would hurt readability because the _Arrange_ section would get quite bulky again.

### Tips for keeping your unit tests lean and understandable

- I usually only 'new up' the class under test directly in the _Arrange_ section, other (fake) objects are created in helper methods or classes.
- Use existing and proven libraries &amp; frameworks so you don't have to write boilerplate code and you can focus on more difficult problems.
    - Try to use a mocking framework (such as [FakeItEasy](https://github.com/FakeItEasy/FakeItEasy)) over a custom made mock/stub framework. Using a custom framework costs time in two ways: it needs to be maintained and it needs to be explained to every new member on the team.
    - Need to create collections of fake objecs? Consider using [AutoFixture's `CreateMany<T>`](http://blog.ploeh.dk/2009/05/11/AnonymousSequencesWithAutoFixture/).
    - The [FluentAssertions](http://www.fluentassertions.com/) library contains dozens of useful assert methods, especially for collections.
- Always be very explicit in naming the methods and arguments to avoid unclarity. Consider using named arguments when you need to pass numbers, strings or null to methods. If you're using code analysis tools such as Resharper you'll get warnings that the usage of named arguments is not required most of the time. You might want to lower the severity of that message so the code analysis stays 'green'.

Happy unit testing!