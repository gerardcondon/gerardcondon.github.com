---
layout: post
title: "Uncle Bob's \"Architecture The Lost Years\" Talk"
date: 2013-04-17 22:46
comments: true
categories: architecture
keywords: architecture, Bob Martin, hexagonal
---

I found this [talk](
http://confreaks.com/videos/759-rubymidwest2011-keynote-architecture-the-lost-years) by Uncle Bob Martin to be really enjoyable and thought provoking. In it he talks about decoupling your application from the database and the web. It's ostensibly a Rails talk but I think it applies to every language. 

It really crystalized the issues I've been having with developing Objective-C applications i.e. the intermixing of the database (specifically Core Data) and UI code. After viewing this, I was able to come to iOS development with a whole new perspective. It's so much clearer to me now how to architect applications, where code belongs and what interfaces are needed. After completely separating Core Data and UI code, the whole application has gotten a lot simpler and is much easier to unit test.

It also introduced me to Alistair Cockburn's [Hexagonal Architecture][]. This is a really nice architecture for isolating the application from external entities such as the GUI or database. I've seen this crop up more and more on various Rails blogs under the term 'Hexagonal Rails'.

As a reference, [here](http://blog.8thlight.com/uncle-bob/2011/11/22/Clean-Architecture.html) and [here](http://blog.8thlight.com/uncle-bob/2012/08/13/the-clean-architecture.html) are a couple of other links from Bob Martin's blog talking about "clean architecture".

[Hexagonal Architecture]: http://alistair.cockburn.us/Hexagonal+architecture



