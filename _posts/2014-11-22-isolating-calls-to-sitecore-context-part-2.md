---
layout: post
title: "Isolating calls to Sitecore.Context for improved unit testability - Part II: ItemAdapter"
tags: Sitecore, .Net, unit testing
canonical: "http://blog.marcduiker.nl/2014/11/isolating-calls-to-sitecorecontext-for_22.html"
---

<img class="u-max-full-width" src="{{ site.url }}/assets/2014/11/22/norris adapt.jpg" alt="I don't adapt to my environment, my enviroment adapts to me.">

## Recap of Part I

This is part two of the "Isolating calls to Sitecore.Context..." series. If you haven't read the [Part I]({{ site.url }}/2014/11/18/isolating-calls-to-sitecore-context-part-1.html) please do so to get the right context (pun intended).

In Part I `the GetItem()` method from `ItemProvider` returned an actual Sitecore Item. Because of the `IItemProvider` interface and Sitecore.FakeDb it is possible to return fake Sitecore items and no dependency to the Sitecore context is required in unit tests.

Although unit testing is now possible there are some (minor) downsides to them due to Sitecore.FakeDb:

1. Unit tests still require additional Sitecore assemblies and the Sitecore license file. 
2. Unit tests look a bit cluttered due to setting up the fake Db and DbItem. 
3. Unit tests are not very fast to execute.

So lets look at another way of dealing with Sitecore items to get very lean unit tests.

<!--more-->
## Adapters

I prefer to use abstractions of Sitecore objects because they make unit testing so much easier. The abstractions act as an adapter. It wraps the Sitecore object and exposes some frequently used properties and methods of that object. The adapter or wrapper pattern in combination with Sitecore is quite common and has been described earlier by several others (e.g. [Alistair Deneys](https://adeneys.wordpress.com/2012/04/13/mocking-sitecore/) and [Martina Welander](http://mhwelander.net/2014/04/30/unit-testing-sitecore-mvc/)). 

So instead of working directly with a Sitecore `Item` we can work with an `IItemAdapter `interface which is implemented by the `ItemAdapter` type.

{% gist b66bbf44623b22693c47 %}

{% gist 95d14b8fb50300bb45b9 %}

Note that the original Sitecore `Item` is accessible through the `InnerItem` property. 

Code that should be unit testable should rely only on the other properties the adapter exposes. Code that requires `Item` properties which are not exposed directly by the `ItemAdapter` (and don't require unit testing) could use the `InnerItem` property.

Let's have a look now at the new `IItemProvider` interface and `ItemProvider `implementation.

{% gist b2259bdb9c888b33cb9e %}

{% gist 523551a33b3c46904b43 %} 

A new method is added called `GetItemAdapter()`. When in the web context the `ItemProvider` will call it's own `GetItem()` method which will return an actual Sitecore `Item` and wrap it in an `ItemAdapter`. In a unit test context however `IItemProvider` will be mocked and the `GetItemAdapter()` method will be set-up to return a fake `ItemAdapter` (i.e. not based on a Sitecore `Item`).

Let's recall the `AuthorProvider` example which was used in part I. Here's the new `AuthorProvider` class where the `GetAuthorItem()` method now calls the `GetItemAdapter()` method of the `ItemProvider` and thus returning an `IItemAdapter`.

{% gist f91518f539e74612177a %}

## Unit tests with IItemAdapter and Moq

Here is the unit test for the `GetAuthor() `method when the `AuthorProvider` works with an `IItemAdapter`.

{% gist 1cd13b5807dcf40ac7df %}

When compared with the unit test in the first post (which used FakeDb) this unit test is slightly more compact and easier to understand. __Don't get me wrong, I really like Sitecore.FakeDb but use it only when you can't use an adapter.__

Let's look at the unit test in more detail.

### // Arrange

- First a new `Id` is generated which will be used for the `IItemAdapter` mock.
- The `GetAuthorItemMock()` method contructs a mock object (`authorItemMock`) based on `IItemAdapter` and requires parameters for the Id, author name and company name.
- The `GetItemProviderMock()` method constructs a mock (`itemProviderMock`) based on `IItemProvider`. The `authorItemMock` is passed as a parameter since that will be the result of the `GetItemAdapter()` method of the mock.
- An instance is created of the `AuthorProvider` and the `itemProviderMock` is passed in the constructor.

### // Act

- The `GetAuthor()` method on the `AuthorProvider` is called. Inside this method the `GetAuthorItem()` method is called which in turn executes the set-up `GetItemAdapter()` method of the mocked `IItemProvider`. A mocked `IItemAdapter` is returned and mapped to a new `Author` object.

### // Assert 

- An assertion is done to check if the Name property of the `Author` object is equal to the author name field of the mocked `IItemAdapter`.

## Conclusion

Creating adapters for Sitecore objects can be a relatively quick way to get unit testable code as long as dependency injection principles are used. You are in complete control of the adapter interface. You can start with a very lightweight interface and just expose a couple of properties you need for proper unit testing. Then you can gradually introduce additional properties to the interface as needed.

Next to the Sitecore `Item`, other frequently adapted Sitecore objects are `Database`, `Context` and `SiteContext`. More of that in a later post.

## Source code

The full source code that belongs to this post (and more) can be found on [Github](https://github.com/marcduiker/SitecorePlayground).
