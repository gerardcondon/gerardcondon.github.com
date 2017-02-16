---
layout: post
title: "Adding sidebar links to local Gitbooks"
date: 2017-02-15 23:39
comments: true
categories: Development
keywords: Gitbook, links
---
I've started keeping track of various notes in markdown format and I wanted to use [Gitbook](https://www.gitbook.com) to group them together.
I'm using the [npm module](https://github.com/GitbookIO/gitbook-cli) to build it locally.
It's a useful tool and theThe completed book looks nice.
However I would like to add some customisations e.g. links back to my main page.

It's not easy to add simple customisations.
Or more like it probably is but it's not well documented how to do so.
Editing the header is a bit awkward as you have to modify the theme yourself.
I'll probably end up just adding a header above it using an iframe.

One thing I did find out how to do was to add links to the sidebar.
You edit the book.json to add a links section like this

``` json book.json
  {
    "links": {
      "sidebar": {
        "Gerard Condon": "http://www.gerardcondon.com",
        "Github": "https://github.com/gerardcondon/tools-docs"
      }
    }
  }
```
