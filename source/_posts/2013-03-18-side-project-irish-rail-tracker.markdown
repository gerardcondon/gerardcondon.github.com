---
layout: post
title: "Side Project: Irish Rail Tracker"
date: 2013-03-18 10:56
comments: true
categories: Projects
keywords: HTML, Backbone, Irish Rail, CSS, REST
---

I've been working on a [side project][] for the last month in order to learn about front-end web development. While not complete by any means, I think it's good enough to release as a beta. I plan to add features, improve the UI and refactor the code in future.

The motivation behind it was that I wanted to do a project using one of the JavaScript [MVC Frameworks][] - for this one I chose [Backbone][]. Also I'll be working more with HTML and CSS in work over the coming year and I need to ramp up on these.

[Iarnród Éireann][] (the Irish Rail company) has a REST API which provides details of the stations in its network and the current status of its trains. My [webpage][] plots these stations and trains on a Google Map and shows upcoming train information for each station.

I plan to write a series of posts on how I implemented this and the lessons I learned along the way.

The project is available [here][] and the code is available on [Github][] under the MIT licence. The roadmap.md document is where I'm tracking future work that needs to be done on the project. Any feedback would be much appreciated.

As far as the code goes:

* I'm happy enough with the Backbone model and collection code. The Backbone view code still needs some refactoring to remove duplication and use proper Backbone idioms. 
* There are some JavaScript [Jasmine][] tests - mostly for the model. I'm still figuring out the best way to test the UI - I have some Jasmine tests but also some Ruby [Capybara][] tests for the browser.
* The HTML and CSS code are functional rather than elegant. I'm hoping to improve these as time goes by and as I get more experience with these languages.

[side project]: http://www.gerardcondon.com/projects/irishrail/index.html
[MVC Frameworks]: http://addyosmani.github.com/todomvc/
[Backbone]: http://backbonejs.org
[Iarnród Éireann]: http://www.irishrail.ie
[webpage]: http://www.gerardcondon.com/projects/irishrail/index.html
[here]: http://www.gerardcondon.com/projects/irishrail/index.html
[Github]: https://github.com/gerardcondon/Irish-Rail-Tracker
[Jasmine]: http://pivotal.github.com/jasmine/
[Capybara]: https://github.com/jnicklas/capybara

