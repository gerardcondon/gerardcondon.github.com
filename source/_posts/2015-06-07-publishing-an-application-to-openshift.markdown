---
layout: post
title: "Publishing an application to OpenShift"
date: 2015-06-07 23:08
comments: true
categories: Development
keywords: OpenShift, Rails, Github
---

I was doing some sample Rails apps recently and was looking for a place to run them. [Heroku][] would have been my first port of call but given the limits on database size for the free account, I looked around to see what else was out there. I ended up on [OpenShift][]. The free account gives 3 gears (or VMs) with 1GB storage on each. This is better for me as I can run a proper database on it without the 10000 row limit. The 1GB storage is also persistent so you can use it to store assets.

Redhat has an [rhc gem][] which allows you to control your gears from the command line. You can create new apps from here or you can do as I did and create them from the OpenShift web page. They have a large list of pre-configured applications covering languages such as Java, Ruby, Python and frameworks like Node and Rails.

I selected the Rails 4 application. This forks the [Rails 4 example][] repository from Github. The name you give your application forms the basis for its url i.e. appname-username.rhcloud.com. You can choose a database - the options for Rails are [MySql][] and [Postgres][]. This creates a blank Rails application - to add OpenShift support to an existing application you can follow the steps [here](http://pranavprakash.net/2014/11/08/migrating-ruby-on-rails-app-from-heroku-to-openshift/).

After a short wait a screen pops up with the database credentials and instructions on how to clone the application to a local git repository.

After I clone the repository I typically change the configuration. I want to also store the application code on Github, so I rename the origin repository to openshift.

     git remote rename origin openshift

Then I add a new origin remote using 

    git remote add origin git@github.com:gerardcondon/rails-app.git

and I do an initial push to this remote

    git push origin

The current master branch is still tracking openshift/master so now I change this to track origin/master using 

    git branch master -u origin/master

Now by default changes are pushed to Github when I run `git push`. When I want to deploy to OpenShift I have to manually specify it using 

    git push openshift

After the push, Openshift will stop the application, perform any db migrations and restart it again. There is also [support](https://developers.openshift.com/en/managing-modifying-applications.html) for adding in your own operations at various stages in the deployment.

If I want to clone the repository on a new machine then I clone from Github and add an OpenShift remote using
    
    git remote add openshift ssh://<<openshift repository ssh url>>

The repository's ssh url is available from the application's dashboard on OpenShift.

[Heroku]: http://www.heroku.com
[OpenShift]: http://www.openshift.com
[rhc gem]:https://rubygems.org/gems/rhc/
[MySql]:https://www.mysql.com
[Postgres]:http://www.postgresql.org
[Rails 4 example]: https://github.com/openshift/rails4-example