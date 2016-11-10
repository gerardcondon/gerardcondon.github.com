---
layout: post
title: "Fixing Twitter sidebar for Octopress 2.x"
date: 2016-11-09 22:02
comments: true
categories: Development
---

I noticed recently that the Octopress Twitter plugin for my sidebar was broken.
The reason is that Twitter no longer supports the APIs that the plugin accesses.
Instead now they have a [widget page](https://publish.twitter.com/#) to generate html code that will embed a list of your tweets on your page.
 
I created a widget for myself and customised it by adding the `nofooter transparent noheader` attributes to `data-chrome`.
These remove various headings and background colouring so that it blends in with the site a bit more.
 
To add this to an Octopress site, you replace the code in `source/_includes/asides/twitter.html` with this new code.
I also added a follow button which displays a Twitter formatted follow button and a count of your subscribers.

The full code is as follows

``` html source/_includes/asides/twitter.html
{% if site.twitter_user %}
<section>
  <h1>Latest Tweets</h1>
  <a class="twitter-timeline" data-chrome="nofooter transparent noheader" data-tweet-limit="5" href="https://twitter.com/gercondon">Tweets by gercondon</a> <script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
  <a class="twitter-follow-button" href="https://twitter.com/gercondon">Follow @gercondon</a>
</section> 
{% endif %}
```