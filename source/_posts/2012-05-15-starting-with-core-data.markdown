---
layout: post
title: "Starting out with Core Data"
date: 2012-05-15 21:59
comments: true
categories: Development
keywords: Core Data, Magical Record, Mogenerator, Parse.com
---

For my project, I need a way to store the model data that I will be generating. I certainly don't want to write a persistence layer for my model objects so I will definitely use a standard library for this. There are a number of options

*	Core Data. This provides an editor to create the objects and XCode will generate the required model classes for me.
*	Use a database like Sqlite directly. Core Data uses a sqlite database behind the scenes but instead I could create one directly and access it using a library like [FMVH][].
*	A service like [Parse.com][]. This provides an online backend for apps.

I don't want to use the database directly as I prefer working with objects, so that would rule out sqlite for me. Parse.com is really interesting. Among other things, it provides an online site where you can see the data in a spreadsheet interface. This [tutorial][] shows how you can use Parse.com to implement a todo list website. It's a real pity that iCloud doesn't provide a similar API to allow non-Apple devices access the data.

In the end I've decided to go with Core Data. The reasons for this are

* It's an Apple standard library and one of the purposes of writing an app was to learn about iOS and Apple technologies. Where possible I've tried to stick to the Apple way of doing things as I can be sure that this approach will be supported in future - or if there is a new system, then a transition path will be provided.
* Core Data works on the object level rather than the database level. I've programmed in objects for most of my career so this is familiar to me. I have a working knowledge of databases but I feel more comfortable using OOP.
* I'm assuming that Core Data should integrate with iCloud better than other approaches. Thinking ahead I'd like to look at iCloud at some stage and learning Core Data first should make that a bit easier.

## Magical Record and Mogenerator

There are some good Core Data tutorials out there. The best introduction that I found was the [Stanford][] iOS course (lectures 13 & 14). To be honest, the code required to implement Core Data is fairly complex and verbose. To deal with that, I started looking for libraries which would improve the Core Data experience. The best I found is [Magical Record][]. This library simplifies Core Data a great deal and provides loads of helper categories & functions. There is an excellent tutorial on MagicalRecord [here](http://yannickloriot.com/2012/03/magicalrecord-how-to-make-programming-with-core-data-pleasant/). I also found [this](http://ablfx.com/blog/?post_type=post&p=561) tutorial really good and it also includes a sample project which uses MagicalRecord.

Magical Record works well with another tool called [Mogenerator][]. You download a .dmg from the site and install from that. There is a nice tutorial [here](http://raptureinvenice.com/getting-started-with-mogenerator/). Mogenerator generates classes from the Core Data xcdatamodel file. The normal Core Data build can also do this but the advantage of Mogenerator is that it generates a second set of classes for each Core Data entity. This second set will never be overwritten so you can write custom code in these. The first set of classes will be regenerated each time the model gets updated.

Magical Record is available on Github. Like most iOS projects I've found there, it seems the standard way to integrate this with your project is to copy the source files into your code and work away. I'm not too comfortable with this approach and would prefer to keep third party libraries separate from my code. This would allow me to update these independently and share them between projects. I'll have to look into what options are available for this. 

[FMVH]: https://github.com/ccgus/fmdb
[tutorial]: http://houseofbilz.com/archives/2011/11/30/an-example-backboneparse-com-app/
[Parse.com]: http://www.parse.com
[Stanford]: http://www.stanford.edu/class/cs193p/cgi-bin/drupal/downloads-2011-fall
[Magical Record]: https://github.com/magicalpanda/MagicalRecord
[Mogenerator]: https://github.com/rentzsch/mogenerator
