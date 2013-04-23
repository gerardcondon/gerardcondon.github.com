---
layout: post
title: "Irish Rail's RealTime API"
date: 2013-04-23 23:03
comments: true
categories: development
keywords: Irish Rail, API, YQL, XML
---

I spent a lot of time in my childhood and early twenties traveling on the Irish Rail Network. Mostly this seemed to involve waiting for ages at Limerick Junction or barely finding a space to stand on a packed train from Dublin. Those experiences didn't exactly leave me with a great impression of [Iarnród Éireann][]. 

Given this, I was amazed to find out recently, that Iarnród Éireann have an [XML API][] for accessing realtime data about the trains running on their network. Credit where credit is due, this is an excellent idea and I wish more companies would implement something similar.

The API provides functionality for getting a list of the stations and what trains are due at those stations in the next ninety minutes. It also gives a list of all trains active on the network. It can filter by DART, Suburban or Mainline trains (that leaves some trains in an 'other' category - I'm not too sure what these actually are). 

I used this API as part of a Backbone learning [project][]. It was quite fun to do. The API returns latitude and longitude coordinates for each station and train, allowing them to be plotted on a Google Map widget. I never realised there were so many stations in Ireland until I saw them plotted on the map.

One issue I ran into was testing the application with live data. Given that I was programming this at night after work, you'd soon reach a time when there are very few trains left on the network!

One technical detail about the API is that it is in XML rather than JSON. This means that I can't use Ajax or JSONP to get the data, due to the same origin policy. Instead I had to bounce the results through [YQL][]. YQL exposes a SQL like interface to web data. I'm only using a basic 'select all' query here but looking at their site you can do lots of cool and complex stuff. I found a good [tutorial][] from Cypress North on how to use YQL in your code to consume an XML API.

[Iarnród Éireann]: http://www.irishrail.ie
[XML API]: http://api.irishrail.ie/realtime/index.htm
[YQL]: http://developer.yahoo.com/yql/
[project]: http://www.gerardcondon.com/projects/irishrail/index.html
[tutorial]: http://www.cypressnorth.com/blog/programming/cross-domain-ajax-request-with-xml-response-for-iefirefoxchrome-safari-jquery/