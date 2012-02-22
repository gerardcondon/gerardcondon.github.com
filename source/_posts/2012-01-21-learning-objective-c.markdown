--- 
layout: post
title: Learning Objective C
date: 2012-01-21 18:00
comments: true
categories: Development
keywords: iOS Development, Apple, learning Objective C
---
Here is a list of the resources I used when learning Objective C.

*	[Apple Documentation](http://developer.apple.com/library/ios/navigation/#section=Resource%20Types&amp;topic=Getting%20Started). This is really good, especially the Javadoc like pages for the UI classes. There are load of documents here. Ones that I missed first time around are the “Your First/Second/Third iOS App” guides. I’ll have to catch up on those. (One tip which I completely missed out on for ages. For these webpages, on the right hand side of the second toolbar, is a button which gets the whole doc as a PDF. It’s worth building up a library of these.) The two main documents that I used were
    *	[The Human Interface Guide](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html). This is a really excellent document for describing how to design an iOS app. It’s not code based but instead operates at a higher level explaining the UI principles and how the UI elements should be used.
	*    [Objective C Programming Language](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html#//apple_ref/doc/uid/TP30001163). I think this document is essential for learning Obejctive C. It details the extensions Objective C makes to the base C language and is really thorough.
*    [Stack Overflow](http://www.stackoverflow.com"). This site is an absolute godsend. How did we ever live without it? The documents above are fine as an introduction to Objective C and app development. However when you start to create an app and run into practical issues, this site is fantastic. There has been no coding question that I have had that they haven’t been able to answer and the coding samples provided are normally top notch. I can’t recommend this site enough.
*	Google & Blogs. In general searching for Objective C questions on google brings up a list of blogs which detail people’s practical implementations of various issues. These are useful to read as they normally contain any design decisions and reasons why alternatives were rejected.

The main class of resources that I haven’t decided to use in learning Objective C have been books. There seem to be some good ones out there but I don’t think there’s much there that I haven’t been able to find on the web. Blogs and Stack Overflow have killed programming language books for me.

The other thing is that I think books about specific programming languages are the ones at risk of going out of date. My data structures and pattern books from college are still useful but any Java books from then have become outdated. The core object-oriented parts are still valid but you can say goodbye to vectors, Swing etc. I’ll have to keep an eye out for good Objective C pattern books.

To give a specific example, the ARC memory management recently introduced to Objective C has rendered the sample code in a lot of books obsolete. Not just obsolete but incorrect actually as the old code won’t compile with ARC enabled. On the web this can be updated, but the books are stuck with the older, invalid code. Maybe with ebooks becoming more popular there will be scope for having books that update their sample code automatically. Until then we have to wait for revised editions, if indeed they are released at all.

I just found [this](http://chachatelier.fr/programmation/fichiers/cpp-objc-en.pdf) document (via [@steipete](http://www.twitter.com/steipete)) which is a guide from C++ to Objective C. This looks to be really good.
