---
layout: post
title: "Durable Functions - The Anatomy of Function Naming"
tags: azure durable functions serverless faas stateful orchestration
---

## Function Names in the Wild

Whenever I see examples or implementations of Azure Funtions I always see this:

 `[FunctionName("myfunction")]`
 
Where the function name is present as a direct string literal. I'm not questioning that it *is* a string because that's simply the way the `FunctionName` attribute works currently. I'm more concerned how the string got there. 

I'd like to show two different ways to refer to function names in a safe and consistent way. This is especially useful when you're using [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview) and need to refer to activity function names in your orchestration function.

<!--more-->

## Literal Strings and Meaning

I admit that I've probably been reading too much *Code Complete* and *Clean Code* in my life because whenever I see literal strings in code I usually get the chills. So I'm probably overreacting a bit...

What I find most annoying about direct usage of literal strings in general is that the intent or meaning is usually ambiguous.

Here a simplified example of what I've seen recently (unrelated to Azure Functions):

`Builder.Create<DomainObject>("Obj1", "Domain object 1")`

When reading this line of code it was not not clear to me what `"Obj1"` and `"Domain object 1"` meant. Are those IDs, names, labels, descriptions? And are they required to be unique or not? When I'm reading code I don't want to play a guessing game.

## Naming is Hard

I definitely agree with [Phil Karlton](https://skeptics.stackexchange.com/questions/19836/has-phil-karlton-ever-said-there-are-only-two-hard-things-in-computer-science) on that one. It often occurs that I rename a class or method just minutes after I created it.

When it comes to naming functions I always start with a verb followed by a noun related to the domain language. I don't mind verbose class or method names as long as they clearly communicate their intent.

I recenty updated my Durable Functions demo code I wrap some calls to [swapi.co](http://swapi.co) (Star Wars API) in activity functions. Those function names are `GetCharacter`, `SearchCharacter`, `GetPlanet` and `SearchPlanet`. I hope you will agree the intent is clear.

## Structuring Orchestration and Activity Functions

I prefer my orchestration and activity functions to be in seperate classes. In adddition, I prefer to have each activity function in it's own class (and file). For me this brings the following benefits:

- Files and classes are kept very small and therefore very readable.
- When I add a new activity function I don't risk 'touching' other functions (except to call it from the orchestration function of course).
- It's very easy to locate the individual functions in Visual Studio.
- I can use the class name as the `FunctionName` identifier! (keep reading...)

## Safe and Consistent Naming

An orchestration function depends on the implemented activity functions and both types of functions are located in the same Function App. This means we don't need to provide string literals seperately for the FunctionName in the activity and in the `CallActivityAsync()` method in the orchestration but we can do something more 'clever'.

### Option 1: Static Naming Class

The most basic option to have consistent naming of functions is to use a static class which contains the names as static fields/properties. Now both orchestration and activity functions can use that naming class. 

{% gist 0a841a123b103acbcd5fe96437b87084 %}

I'd suggest this option only if you require a function naming convention that is not compatible with your C# class naming convention (e.g. you require hyphens or dots in the function name).

### Option 2: The Power of nameof()

Since I write only one function per class I do the following when specifying the `FunctionName` attribute:

{% gist 73c287a5d07c9c8a2a881cc7ecf25960 %}

The `nameof()` expression is available since [C# 6.0](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) and provides a very clean way to use strings based on class names.

The benefits are that I can now refer to an activity function in a strongly typed fashion so renaming/refactoring is less fragile.

The downside of this technique is ofcourse that function names are limited to valid C# class names.

## Which one do you prefer?

So there you go, two options to refer to your function names in a safe and consistent manner when using Durable Functions. 

I'd like to know which one do you prefer, so feel free to reach out to me on [Twitter](https://twitter.com/marcduiker) or [GitHub](https://github.com/marcduiker/demos-azure-durable-functions/issues).

## Resources

The full souce code with various demos about Durable Functions can be found in my [demos-azure-durable-functions repo on GitHub](https://github.com/marcduiker/demos-azure-durable-functions).
