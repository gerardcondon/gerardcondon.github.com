---
layout: post
title: "The Design of iCloud"
date: 2013-06-04 22:35
comments: true
categories: development
keywords: iCloud, Core Data, Dropbox, Azure, Amazon
---

There's been a lot of blog posts from developers recently about the problems with iCloud syncing. The Verge has a great summary [here](http://www.theverge.com/2013/3/26/4148628/why-doesnt-icloud-just-work). There are quite a few who are removing iCloud from their products and going with other syncing options such as Dropbox.

The impression I get from these discussions is that it's the reliability of iCloud that's the problem, i.e. if iCloud was rock solid then it would be a great option for your app. I disagree with this view and I think the design of iCloud is fundamentally flawed.

I think that even if iCloud Database syncing was perfectly reliable, it would still be a bad way of syncing data. One conclusion I've drawn from looking at web backends and Rails in particular, is that iCloud is only useful if you want to stick to Apple devices. There is no way to get at this data outside of iOS or OS X. In particular it is impossible to access this from a web application. For this reason, I think it's vital to have a proper backend if you are storing data in the cloud.

The other mode of iCloud syncing is the Document based syncing. The issue I have with this is that anything stored in iCloud is restricted to the application that created it. This is a major issue when an application stores data in a common file format (e.g. plain text or image formats such as PNG or JPEG) that you may expect to be able to use in another application. Dropbox is a far superior solution in this case. I feel much more confident in the apps which use this over iCloud, as I will always have access to the data files.

The other day, Brent Simmons [posted](http://inessential.com/2013/05/07/30_minutes_to_sync) a great proposal for an Apple backend service. They really need to do something here as their competitors aren't standing still. Microsoft is on the right track here with Azure and similarly Amazon's cloud computing services are going from strength to strength. It will be interesting to see if anything will be announced for iCloud at WWDC.
