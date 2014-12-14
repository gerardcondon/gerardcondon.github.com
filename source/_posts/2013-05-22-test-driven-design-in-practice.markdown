---
layout: post
title: "Test Driven Design in practice"
date: 2013-05-22 21:35
comments: true
categories: Development
keywords: TDD, JavaScript
---

I recently tried implementing a JavaScript project at work using the testing methods I've learned from the [Destroy All Software] Screencast. It ended up being some of the best code I've written. The interfaces grew neatly, it wasn't not over designed and it was completely covered by tests. It's the project that I have the most confidence in its correctnesss. It's nice to know that despite any modifications in future, as long as all the tests pass it will pretty much always work first time.

Anywhere I've worked up to now, testing was always seen as something that you implemented after the fact. Code coverage was the main driver of the testing. However this approach completely misses the input that TDD has on the design of the application. By writing applications so that they can be tested easily, they turn out to be much better designed. They are less coupled and all the dependencies are visible. Having the design emerge from the growing system is better than imposing over elaborate architecture and patterns top down.

I found a couple of good resources recently on testing and the impact it has on your code. This is a good [talk][] by Michael Feathers on the synergy between testing and design. He shows how testing problems are indicative of design problems. Misko Hevery's [site][] has some great presentations and resources on how to design code that is testable. 

[talk]: http://vimeo.com/15007792
[site]: http://misko.hevery.com
[Destroy All Software]: http://www.destroyallsoftware.com

