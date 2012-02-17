--- 
layout: post
title: Devising my Unit Testing Philosophy
date: 2012-02-10 18:00
comments: true
categories: Development
---
Up to now my approach to unit testing has been dictated by who I was writing the code for. For instance in college I never wrote unit tests. In the real world some of my employers required unit testing, with certain branch and code coverage goals, while other employers didn’t require much in the way of unit testing, as long as other types of testing were carried out. In practice I found that the as the code became more lower level, the demands for unit tests increased.

For this project I need to define for myself what level of testing I will perform. My initial assumption was that of course I would do unit testing. Personally I thought that the benefits of unit testing were

*   the confidence that it gives me in my code. I know that I can call this piece of code from somewhere else and there is a high probability that it will work as intended.
*   by catching errors at an early stage, I spend less time in the debugger later.
*   refactoring should go easier as the unit tests will catch a lot of regression issues
*   I can be confident that any new bugs will be in the new code rather than the older highly unit tested modules, thus cutting down the amount of code required to search in order to fix the bug.

However I read an interesting and thought provoking [article](http://wilshipley.com/blog/2005/09/unit-testing-is-teh-suck-urr.html") by Will Shipley outlining why he doesn’t do unit testing. My immediate reaction was astonishment. How can you not have unit tests? However instead of unit tests, he beta tests the product. This finds the type of bugs that the actual end users will find. Also he writes his code in a manner that doesn’t require unit tests.

> My programming philosophy is “less code is better code.” Unit tests take a lot of code, and in my projects I don’t find that they find very many bugs. Part of this is because I tend not to modify my individual methods much once they’ve been written; if my “append string to string” method works, it’s really not going to get revisited anytime soon unless it has a bug.
> Part of it is because, in fact, I do do integrated testing of a form, and I probably should have talked about that more. Yes, I use “NSAssert” all over the place, and I perform sanity checks and raise errors if there’s a problem.

I don’t think I’ve come across “Not modifying methods once they are written” before but looking at it now it makes sense. By not changing existing working methods and treating them as black boxes, we should have fewer regression errors in future. I’m not sure how to reconcile this with refactoring though - it’s something to think about. I’d imagine a coding style that favours smaller, more focused methods would be more suited to this approach.

His use of sanity checks and asserts also struck a chord especially when he says
> Debugging most errors, once found, should take very little time in a properly-written program, because the error should always be from one of your own sanity checks, and you know where those are in the code, so you just go there and figure out what went wrong.

This was eye-opening for me. I’ve never tracked this before so I’m not sure if the bugs I’ve written have evaded my sanity checks and asserts. I would imagine a lot of them did. I’d never have seen that as important before but looking at it now it seems to me that this is a failure on my behalf. Not catching these errors means that I’m not fully aware of how my code can go wrong and what it is doing in all cases. I think a useful analysis I should run on each bug I find is - did this escape my coding checks/asserts and if so why? I should learn from this and in future code should check for this condition.

In a response to Will’s article, a [follow up post](http://www.friday.com/bbum/2005/09/24/unit-testing) by bbum shows how unit testing was of great benefit on infrastructure code, in this case the [Core Data](http://developer.apple.com/macosx/coredata.html) library. This would suggest to me that the utility and model code would be amenable to unit testing but that it’s not useful for the GUI code.

Another [article](http://blog.codeville.net/2009/08/24/writing-great-unit-tests-best-and-worst-practises/) on codeville challenged my assumptions that unit tests were about finding bugs. Instead the type of testing that finds bugs is manual testing or automated integration testing. According to him, unit testing is not about finding bugs (except during refactoring) but more about designing robust software components.
> TDD is a design process, not a testing process. TDD is a robust way of designing software components (“units”) interactively so that their behaviour is specified through unit tests
Also it’s not enough to just write unit tests. The tests need to be either true unit tests which design a single component or else integration tests which automate the entire system. Anything in between are
> dirty hybrids. Unclear goal. High maintenance, don’t prove much
Otherwise when we refactor code, we end up breaking lots of seemingly unrelated hybrid tests. If you change a unit, then its unit tests should change but no other unit tests should.

After all that I’ve decided that I want to do some level of unit testing but I realize that I need to focus it on the right areas. I think it will be useful for the utility type code that I write but that I need to do more research into how I want to implement integration testing and UI testing.
