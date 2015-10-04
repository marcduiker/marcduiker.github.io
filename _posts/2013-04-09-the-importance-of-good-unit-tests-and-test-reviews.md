---
layout: post
title: The Importance of Good Unit Tests and Test Reviews
---

{% include header.html %}

#{ page.title }
_2013-04-09 Marc Duiker_

---

I'm currently following an [online TDD course](https://www.udemy.com/draft/14162/) by [Roy Osherove](http://osherove.com/). I'm about half way through and although I have quite a bit of experience writing unit tests and using test frameworks I've gained a lot of knowledge from the course already. Here are some highlights about good unit tests and test reviews.

##Good Unit Tests
Roy stresses the importance of the three pillars of a good unit test:

1. Readability
2. Maintainability
3. Trustworthiness

If any of these are not taken into account during development developers are likely to drop unit tests all together because it will become a burden to use instead of an aid.

##Unit Test Reviews
In one of the lessons Roy talks about the importance of test reviews. Having a high code coverage doesn't guarantee your code is good because you could be testing the wrong thing. Doing test reviews can help in ensuring the unit tests are correct. The unit tests themselves should be easy to read and understand to ensure maintainability.

##Guidelines for Writing and Reviewing Tests

1. Unit tests should be easy to locate in the solution. Are the unit tests in a separate project?
2. Readability is key in unit testing. Is there a clear naming convention?
 - Naming the unit test projects:

    `<ProjectUnderTest>.UnitTests`    
    (e.g. MyCompany.MyProduct.Business.UnitTests)    

 - Naming the unit test classes:
  
    `<ClassUnderTest>Tests`    
    (e.g. ShoppingBasketTests for the ShoppingBasket class)    

 - Naming the unit test methods:

    `<UnitOfWork>_<StateUnderTest>_<ExpectedBehavior>`    
    (e.g. CalculateDiscount_ForTenProducts_ReturnsDiscountOf5Percent see [Roy's blog](http://osherove.com/blog/2005/4/3/naming-standards-for-unit-tests.html) for more examples)    

3. Logic in unit tests should be avoided. Are there any if/else, switch, try/catch or loop statements in the test? Then refactor the test into separate tests.
4. Unit tests should be isolated and should always return the same result. Does the test have any dependencies to things beyond your control? (e.g. database, filesystem, DateTime, GUID)
5. A unit test should only test one thing. Are there several asserts in a unit test? Verify if you really need all those asserts. If you do, move them to individual unit tests.

More guidelines on unit test reviews are given [here](http://artofunittesting.com/unit-testing-review-guidelines/). In addition I can highly recommend reading ['The Art Of Unit Testing'](http://www.manning.com/osherove/). The second edition of this excellent book is expected in Q3 of this year.