---
layout: post
title: "Controllers in Objective C"
date: 2013-03-17 13:52
comments: true
categories: Development
keywords: Objective C, MVC, Controllers
---

One of the things to learn about iOS is the MVC model. The Cocoa implementation of MVC has some [differences][] compared to the traditional approach. For example, in Cocoa, views are not aware of the models and don't listen for model updates. Instead the all events pass through the controller, i.e. it listens for model changes and then tells the view to update itself.

In [Rails][], the constant refrain is that controllers should be thin. However on iOS they seem to be absolutely huge. One joke that I saw on Twitter was that on iOS, MVC stood for Massive View Controller. For example, the Recipe sample application from Apple has controllers with hundreds of lines of code with one topping out at 600 LOC.

One of my issues with these view controllers is that they don't follow the Single Responsibility Principle, but instead combine multiple functions. They act as delegates for multiple protocols e.g. table data source, fetched results controller delegate etc. I find it hard to distinguish the separate elements of MVC when one class is doing everything. Also in Objective C, once everything is in the same file, it's not obvious to which protocol an item belongs. I think this risks breaking the MVC boundaries. For example, during a refactor, if you're not careful, you can easily get model variables depending on controller variables and vice-versa.

I'd much prefer it if these controllers were split out into lots of different classes, each with a single job as per the SRP. This would lead to a more composition-based rather than inheritance-based codebase. I also think that this greatly helps with code navigation. Jumping to a small, focused file has the effect of filtering out irrelevant code. I've started using Sublime Text recently and it has great functionality for navigating between files, so I prefer having lots of smaller files rather than a few large monolithic classes.

(On a side note this is one thing that really annoys me about XCode. Given a properly nested folder structure with well named files, I think it becomes a lot easier to find your way around the app. For example even after only a few weeks learning Rails, I know exactly where to look to find the controllers, models, db code etc. But XCode is a disaster here. It doesn't push the groupings made in the app on to the file system underneath. It requires duplicate effort of organising the code both inside and outside the application to keep the codebase properly organised.)

[differences]: http://developer.apple.com/library/ios/documentation/general/conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html#//apple_ref/doc/uid/TP40010810-CH14-SW9
[Rails]: http://rubyonrails.org/
