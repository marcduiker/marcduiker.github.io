---
layout: post
title: "Durable Functions - The Anatomy of Function Naming"
tags: azure durable functions serverless faas stateful orchestration naming
---

<img class="u-max-full-width" itemprop="image" src="{{ site.url }}/assets/2018/06/21/functionnaming.png" alt="How to name your Azure Functions">

## Function Names in the Wild

Whenever I see examples or implementations of Azure Functions I always see this:

 `[FunctionName("myfunction")]`
 
Where the function name is present as a direct string literal. I'm not questioning that it *is* a string because that's simply the way the `FunctionName` attribute works currently. I'm more concerned how the string got there. 

I'd like to show two ways to refer to function names in a safe and consistent way. This is especially useful when you're using [Durable Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-overview) and need to refer to activity function names in your orchestration function.

<!--more-->

## Literal Strings and Meaning

I admit that I've probably been reading too much *Code Complete* and *Clean Code* in my life because whenever I see literal strings in code I usually get the chills. So I'm probably overreacting a bit...

What I find most annoying about the direct usage of literal strings in general is that the intent or meaning is usually unclear or ambiguous (aka [magic string](https://en.wikipedia.org/wiki/Magic_string)). 

Although the meaning of the string in the `[FunctionName]` attribute is clear, what valid input is for the name is not clear. You need to dig into the [FunctionNameAttribute.cs](https://github.com/Azure/azure-webjobs-sdk/blob/9f96d3f1e63ae1241431990f256f1b2e6880167f/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs#L34) class to find the regex that validates the `[FunctionName]` attribute string:

 `^[a-z][a-z0-9_\-]{0,127}$(?<!^host$)` 

... and then you need to understand the regex as well ;).

In addition, you will only be notified of an invalid `[FunctionName]` attribute during runtime: 

`"Orchestrator function 'HelloName' failed:` 
`The function 'Hello.Activity' doesn't exist, is disabled, or is not an activity function.` 
`The following are the active activity functions: '...'"`

If we could use a type-safe way of naming functions, invalid names could be detected much earlier.

## Naming is Hard

I definitely agree with [Phil Karlton](https://skeptics.stackexchange.com/questions/19836/has-phil-karlton-ever-said-there-are-only-two-hard-things-in-computer-science) on that one. It often occurs that I rename a class or method just minutes after I created it.

When it comes to naming functions I always start with a verb followed by a noun related to the domain language. I don't mind verbose class or method names as long as they clearly communicate their intent.

For example, I recently updated my Durable Functions demo code where I wrap some calls to [swapi.co](http://swapi.co) (Star Wars API) in activity functions. Those function names are named `GetCharacter`, `SearchCharacter`, `GetPlanet` and `SearchPlanet`. I hope you will agree the intent is clear.

## Structuring Orchestration and Activity Functions

I prefer my orchestration and activity functions to be in separate classes. In addition, I prefer to have each activity function in its own class (and file). For me this brings the following benefits:

- Files and classes are kept very small and therefore very readable.
- When I add a new activity function I don't risk 'touching' other functions (except to call it from the orchestration function of course).
- It's very easy to locate the individual functions in Visual Studio.
- I can use the class name as the `FunctionName` identifier! (keep reading...)

## Safe and Consistent Naming

An orchestration function depends on the implemented activity functions and both types of functions are located in the same Function App. This means we don't need to provide string literals separately for the `[FunctionName]` in the activity and in the `CallActivityAsync()` methods in the orchestration but we can do something better.

### Option 1: Static Class with Constants

The most basic option to have consistent naming of functions is to use a static class with string constants. Now both orchestration and activity functions can use that naming class:

{% gist 0a841a123b103acbcd5fe96437b87084 %}

I suggest using this option only if you require a function naming convention that is not compatible with your C# class naming convention (e.g. you require hyphens or underscores in the function name). 

You could add comments what valid function names are and, even more important, you should add unit tests that fail when invalid names are used. Then at least you know during your CI/CD process that something is wrong.

### Option 2: The Power of nameof()

Since I write only one function per class I do the following when specifying the `FunctionName` attribute value:

{% gist 73c287a5d07c9c8a2a881cc7ecf25960 %}

The `nameof()` expression is available since [C# 6.0](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) and provides a very clean way to use strings based on class names.

The benefits are that:
- I can now refer to an activity function in a strongly typed fashion so renaming/refactoring is less fragile.
- I always use names that are valid function names.
- I don't have to think about creating __two__ names (class and function), just __one__.

The downsides are that:
- Function names are limited to valid C# class names.
- There can only be one function method per class.

The benefits definitely outweigh the downsides for me.

## Which one do you prefer?

So there you go, two options to refer to your function names in a safe and consistent manner when using Durable Functions. 

I'd like to know which one do you prefer (or maybe you have an alternative), so feel free to reach out to me on [Twitter](https://twitter.com/marcduiker) or [GitHub](https://github.com/marcduiker/demos-azure-durable-functions/issues).

Related to this subject is this issue on Github: [Convention-based FunctionName & HttpTrigger Routes](https://github.com/Azure/azure-functions-core-tools/issues/257).

## Resources

The full souce code with various demos about Durable Functions can be found in my [demos-azure-durable-functions repo on GitHub](https://github.com/marcduiker/demos-azure-durable-functions).
