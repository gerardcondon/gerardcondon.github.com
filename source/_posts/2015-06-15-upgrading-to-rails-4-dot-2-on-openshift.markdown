---
layout: post
title: "Upgrading to Rails 4.2 on OpenShift"
date: 2015-06-15 00:03
comments: true
categories: Development
keywords: Rails, OpenShift
---

I updated my Rails test app from 4.1 to 4.2. However when I pushed to [OpenShift][] I got the following error

    You have already activated rack 1.5.2, but your Gemfile requires rack 1.6.0. 
    Using bundle exec may solve this. (Gem::LoadError)

I found the answer on Google Groups [here](https://groups.google.com/forum/#!topic/phusion-passenger/BLf0FDyhFAM). The root cause is that OpenShift is [dependent](https://bugzilla.redhat.com/show_bug.cgi?id=1184179) on Rack 1.5.2 and Passenger 4.0.18. The proper fix will have to wait until they update their versions of that software. Until then to fix this error, ssh into the OpenShift app and in the `app-root` folder run 

    gem install rack

[OpenShift]: http://www.openshift.com