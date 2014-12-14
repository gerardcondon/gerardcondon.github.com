---
layout: post
title: "Book Review: Beginning Backbone by James Sugrue"
date: 2014-10-02 22:49
comments: true
categories: 
keywords: development
---
My current project at work is a large scale [Backbone][] application. The company had no prior experience in web programming before this project and was mainly used to programming in Java. So in order to staff up the project, we needed some way of converting our Java programmers into Backbone programmers.

To do this we looked at the various training materials available on the web. There are a number of excellent resources out there, such as Addy Osmani's [book](http://addyosmani.github.io/backbone-fundamentals/), the MVC Todo [app](http://todomvc.com/architecture-examples/backbone/) and the Backbone [docs](http://backbonejs.org) themselves. We wanted to develop a selection of documentation/training materials that we could hand to a new member of the team to get them up to speed.

One of the newer books that we've looked at is [Beginning Backbone][] by James Sugrue.
__Disclosure:__ I've previously worked on the same team as James for two years at my current company.

### JavaScript and Backbone Introduction

The book begins with a good introduction and overview of Backbone from an architectural point of view and gives examples of companies who have built products on Backbone. I liked this approach, as it's one thing explaining why you should use Backbone from a coding perspective but it's also nice to be able to justify the choice from a risk perspective to management. Having concrete examples of successful companies helps us make that case.

There is a chapter on JavaScript which is probably obligatory in a book like this. It's fine as an introduction to the language, but you would need to combine this with something specifically for JavaScript like ["Eloquent JavaScript"][] or ["JavaScript: The Good Parts"][].

Each of the components of Backbone is dealt with comprehensively. The models, collections, views, events and router are explained with plenty of examples. Templating described alongside the views using both [Handlebars][] and [Mustache][].

After the introduction we get a walkthrough of how to create an application. The application is surprisingly comprehensive. It's a Twitter clone and not the standard todo app. It deals with linking models to views, reusing views,  and how to tie it together with events.

### Backbone EcoSystem

From there the book branches out to cover the wider Backbone ecosystem. Backbone is not an all encompassing framework. In fact it quite a simple framework with a lot of scope for customisation. It is a foundation upon which you will layer many plugins and libraries, and so understanding what additional addons are available and how to use them is vital to getting the most out of Backbone.

The book covers

* UI considerations such as two way data binding using [Stickit][], layout management using [LayoutManager][] and more.
* Model concerns such as [validations][], [view-models][], [undo-redo][] and others.

We had started coding well before the book was written and a lot of the choices we had made on Backbone plugins are mentioned in the book. It was nice to get some validation of those choices. In addition reading this section of the book prompted us to look at introducing view models to our code.

One of the problems we had was that it's easy to see how a simple todo application can be built from Backbone, but it's harder to extrapolate from there and design how a larger application should hang together. We encountered problems at scale e.g. managing views and their resources when having high double digit numbers of views and templates. The book introduces two plugins - [Marionette][] and [Thorax][] - which extend Backbone to give more comprehensive view management. Even if the specific plugins described in the book are not for you, at least you will be made aware of the issues that await in the future.

As an aside, it is here that the book encounters one of the curses of JavaScript programming - the choice of two equally plausible alternatives! This has been the bane of my life for the past couple of years. For every situation that you come across, there will be two equally valid options. You will have to make a choice between them but you won't have enough information at the time to understand their pros and cons. Murphy's law dictates that of course you will pick the wrong one! You have to choose and second guess yourself for the rest of the project!

The specific JavaScript cases in the book are Marionette/Thorax and QUnit/Jasmine. This isn't just limited to JavaScript. For example, in Rails you have the choice between the omakase and prime [stacks](http://words.steveklabnik.com/rails-has-two-default-stacks). It's beyond the scope of the book to give definitive answers on which to choose. You need to evaluate the options based on your own situation but I think the book gives a good enough head start.

### Building a JavaScript application - TDD, Build Systems

The book is not just about beginning Backbone programming. It is much more than that. It gives you a solid base from which to start developing JavaScript applications. Topics such as testing and automation, building, code management are dealt with.
The benefits of TDD are explained along with an introduction to two of the most popular JavaScript TDD frameworks - [QUnit][] and [Jasmine][].

The book also has a chapter on best practices & design patterns. The emphasis is not just about using Backbone but using it well. It covers user visible features such as performance and memory leaks as well as development concerns such as creating and maintaining a manageable code base. For example, JavaScript modularity is not straightforward. The JavaScript language does not provide for a way for files to include other files. As a programmer you definitely want to split your codebase into separate files and then compile them together for the released product. The book uses [RequireJS][] to show you how to do this.

### Negatives

The formatting of the code samples is off in quite a few places. The indentation is out and there are some spaces missing turning `var myarray` into `varmyarray`. These are more than just code formatting errors - these would lead to compile errors in the code. There is a Github [repository][] of the code samples in the book though, which partially makes up for this.

From a personal perspective, I don't like the [Grunt][] approach to build management so I wasn't too keen on the whole chapter devoted to this. We tried Grunt on our project at work and found that as as the number of build steps increases, the json required to configure Grunt becomes more and more complex. I prefer using code over configuration files as then I have a chance to debug the build process, insert print statements etc. I think there must be better tools out there. Stepping away from JavaScript and using Ruby, there is [Rake][], which is what we use on our project. If you are using Rails then you have the Asset Pipeline approach. I found that Grunt was hard to debug and it was not easy to figure out what went wrong for some step in the middle.

### Summary

Overall I would recommend this book. I think its invaluable for ramping up new developers to a Backbone project.  Also from an experienced programmer's perspective, it is an easy and quick way to gain a broad understanding of the Backbone landscape. It introduces a number of topics, not just Backbone, but also JavaScript development in general. The book promotes a professional and structured approach to software development, making it suitable for a team who are beginning web development and want to get their process set up correctly.

[repository]: https://github.com/jamessugrue/beginning-backbone
[Grunt]: http://gruntjs.com
[Rake]: https://rubygems.org/gems/rake
[Backbone]: http://backbonejs.org
[QUnit]: http://qunitjs.com
[Marionette]: http://marionettejs.com
[Thorax]: http://thoraxjs.org
[Stickit]: http://nytimes.github.io/backbone.stickit/
[LayoutManager]: https://github.com/tbranyen/backbone.layoutmanager/wiki
[validations]: https://github.com/fantactuka/backbone-validator
[view-models]: https://github.com/tommyh/backbone-view-model
[undo-redo]: https://github.com/derickbailey/backbone.memento
[Handlebars]: http://handlebarsjs.com
["JavaScript: The Good Parts"]: http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742
[Jasmine]: http://jasmine.github.io
["Eloquent JavaScript"]: http://eloquentjavascript.net
[Beginning Backbone]: http://www.jamessugrue.ie/softwaredev/beginning-backbone-my-first-book
[RequireJS]: http://requirejs.org
[Mustache]: http://mustache.github.io
