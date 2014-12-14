---
layout: post
title: "Automating Jasmine Unit Tests"
date: 2013-07-10 23:48
comments: true
categories: Development
keywords: JavaScript, testing, PhantomJS
---

For the first cut at automating my JavaScript unit testing, I started running them from the command line via [PhantomJS][]. PhantomJS is a headless browser so it will render my HTML & CSS and execute the JavaScript, but will not display it on the screen. The steps I followed were:

* I installed phantomjs from [here](http://phantomjs.org/download.html) using homebrew `brew install phantomjs`.

* I got the command for running Phantom.js [here](
http://kilon.org/blog/2013/01/running-jasmine-tests-with-phantomjs/).

* I found the default output from Phantom.js to be lacking in detail. I came across a good link [here](http://blog.jphpsf.com/2012/10/31/running-Jasmine-tests-with-Phantom-js-or-Webdriver) which shows how to add stack traces on failure and how to add colours to the output using a console reporter.

In future, I'd like to add this to a build system which will run jshint on my code and also do whatever minfication/optimizations are needed. It's looking like [Grunt][] is a good tool for this so will investigate that further.

[Grunt]: http://gruntjs.com/
[PhantomJS]: http://phantomjs.org/