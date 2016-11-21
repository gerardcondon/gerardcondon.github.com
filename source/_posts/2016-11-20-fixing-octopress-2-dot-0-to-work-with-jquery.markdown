---
layout: post
title: "Fixing Octopress 2.0 to work with JQuery"
date: 2016-11-20 21:44
comments: true
categories: 
---
After updating the site to add [JQuery](www.jquery.com), I noticed that the Github aside and the sidebar toggle no longer worked.

Opening up the console I saw the following errors
    TypeError: Object 0 has no method ‘charAt’
and for the [Github](www.github.com) plugin
    `Method JSONP is not allowed by Access-Control-Allow-Methods`
   
Googling for these lead me to Daniel Hilgarth's blog where he has two posts on how to solve the [charAt](http://blog.fire-development.com/2013/03/06/fixing-octopress-when-using-jquery/) and [jsonp](http://blog.fire-development.com/2013/03/05/fixing-octopress-github-plugin-when-using-jquery/) issues.



   