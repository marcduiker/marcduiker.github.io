---
layout: post
title: "Isolating calls to Sitecore.Context for improved unit testability - Part I: ItemProvider, Moq and FakeDb"
tags: Sitecore, .Net, unit testing
---

![I find your lack of unit tests disturbing!]({{ site.url }}/assets/2014/11/18/vader unit tests.jpg)

##Sitecore projects and (un)testable code

Over the last years I've been involved with quite some Sitecore projects, some were true greenfield projects where a solution is created from scratch and some involved 'only' customizing components or extending the existing platform with new functionality. I enjoy both types of projects since they each have their challenges. I do want to share my concern from what I've seen in some of the latter solutions. Some things that all of these projects had in common were:

1. Little to no utilization of an ORM, such as [Glass](https://marketplace.sitecore.net/Modules/Glass_Sitecore_Mapper.aspx?sc_lang=en) [Synthesis](https://github.com/kamsar/Synthesis), [CDM](https://marketplace.sitecore.net/en/Modules/Compiled_Domain_Model.aspx) (or a well defined self made solution). 
2. Lack of proper testable code (no dependency injection). 
3. Lack of unit tests

Of course all these three points are related. If maintainability is important it is vital to any software project that code is written in such a way that it is unit testable. Although this post concerns isolating Sitecore, it could as well be about isolating calls to a custom database or to a logging component.

This post is intended as a practical guide for the ones involved with these 'difficult' projects and are strongly in favor of improving the code base in order to improve the testability and maintainability without spending many man months up front to make it happen.

##Isolating calls to the Sitecore context

The biggest problem I noticed with some Sitecore solutions is that calls to  `Sitecore.Context.Database.GetItem()` are all over the place. 

The first thing that can be done is to isolate these calls and put this in a custom `ItemProvider` class. (Note that Sitecore has its own `ItemProvider` class in the Sitecore.Kernel.dll but we're not touching that one.) 
So let's start with the following very basic interface (`IItemProvider`) and implementation (`ItemProvider`). It will get more interesting later, I promise.

{% gist 2b15f8eff29303113816 %}

{% gist 2cc9c3add8ef2c0180dd %}

In every Sitecore solution C# models are used which are based on Sitecore templates. Let's assume we are dealing with the following `Author` object in C#.

{% gist 44a8d134e471d1fd023a %}

`Author` instances are usually retrieved via a specific provider such as the `AuthorProvider` below. (The class name in the gist below is a bit longer because I'll show another flavor of this provider in a next post).

{% gist 99b5c1f8a45f6b1adff5 %}

Notice that the constructor of `AuthorProvider` requires an instance of a type that implements `IItemProvider` (this is an example of constructor injection). The `GetAuthorItem()` method calls the `GetItem()` method on the `IItemProvider` and this construction enables us to unit test the `AuthorProvider` using [Sitecore.FakeDb](https://github.com/sergeyshushlyapin/Sitecore.FakeDb) and [Moq](https://github.com/Moq/moq4) (or any other mocking framework you prefer).

##Unit tests with Sitecore.FakeDb and Moq

Sitecore.FakeDb is a very nice unit testing framework which allows creation and manipulation of Sitecore items in memory.It's quite easy to get started with. Just install the [NuGet package](https://www.nuget.org/packages/Sitecore.FakeDb/) and follow the [instructions](https://github.com/sergeyshushlyapin/Sitecore.FakeDb/wiki/Installation) carefully because some Sitecore & Lucene assemblies and a valid Sitecore license are required. In the example below I only use FakeDb as a source for getting Sitecore items.
[Moq](http://www.nuget.org/packages/moq) is a very popular mocking framework. If you don't know it make sure you read at least the [quickstart[(https://github.com/Moq/moq4/wiki/Quickstart).

Here is the unit test for the `AuthorProvider.GetAuthor()` method.

{% gist 1d1ee9a16436501eaaae %}

The goal of the unit test is to verify if  the `GetAuthor()` method of the `AuthorProvider` returns an Author object when a Sitecore item Id is passed in as a parameter. When the `AuthorProvider` is used in a website context an `ItemProvider` is passed into the constructor of the `AuthorProvider` and the item is retrieved from the Sitecore context. In the unit test however we don't want any dependency on the Sitecore context. Therefore an mock is created based on the `IItemProvider `interface. We can set-up the `GetItem()` method on the mock to return a fake Sitecore item which we will get from Sitecore.FakeDb.

Let's look at the the unit test in more detail.

###// Arrange
 
- Since FakeDb is an in memory database an instance is created inside a using statement. This ensures that the in memory database is disposed properly after running the unit test.
- In order to create a new `DbItem` (from Sitecore.FakeDb) an item Name, Id and Template Id are required. The `GetFakeAuthorDbItem` method constructs the `DbItem` with fields for the author name and the company.
- Once the `DbItem` is created and added to the FakeDb instance we retrieve the Sitecore item (`fakeAuthorItem`) from FakeDb.
- Next an `itemProviderMock` object is created based on the `IItemProvider` and the `fakeAuthorItem` is passed since that is used as the returning item for the mocked `GetItem() `method (see the  `GetItemProviderMock` method how that is set-up).
- In the final line of the Arrange section an instance of the `AuthorProvider` is created and the `itemProviderMock` is passed in the constructor.

###// Act

- The `GetAuthor()` method on the `AuthorProvider` is called. Inside this method the `GetAuthorItem()` method is called which in turn executes the set-up `GetItem()` method of the mocked `IItemProvider`. A Sitecore item (from FakeDb) is returned and mapped to a new `Author` object.

###// Assert

- An assertion is done to check if the Name property of the `Author` object is equal to the author name field of the Sitecore item.

##Conclusion

Though this example is fairly straightforward it demonstrates how to write testable code when you're dealing with Sitecore projects. Writing testable code and using a mocking framework in combination with Sitecore.FakeDb in unit tests can be a bit of a learning curve but I consider these as must have skills for any Sitecore developer these days.

In the [next post]({{ site.url }}/2014/11/22/isolating-calls-to-sitecore-context-part-2.html) I'll show a similar approach with an `ItemProvider` that uses an `ItemAdapter` instead of a regular Sitecore item. 

##Source code

The full source code that is used in this post (and lots more) is on [GitHub](https://github.com/marcduiker/SitecorePlayground). Feel free to poke at it and suggest improvements.