---
layout: post
title: "Setting up a Blog on Octopress and Github"
date: 2012-03-04 13:39
comments: true
categories: Octopress
keywords: Octopress, Wordpress, Ruby, Github
---
This post documents how I migrated the blog from [Wordpress.com](http://www.wordpress.com) to [Octopress](http://www.octopress.org). There are a number of bloggers who have written about switching their blogs to Octopress. I found these posts very helpful - [Scott Muc](http://scottmuc.com/migrated-to-octopress/), [Matt Gemmell](http://mattgemmell.com/2011/09/12/blogging-with-octopress/) and [Bigdinosaur Blog](http://blog.bigdinosaur.org/setting-up-octopress/). The basic sequence of steps I followed was

* Install Ruby.
* Install Octopress.
* Create account on [Github](http://www.github.com).
* Export previous blog from Wordpress and convert it to Markdown
* Generate Octopress blog and publish it to Github.
* In parallel I registered www.gerardcondon.com and set it up so that it pointed to the blog on Github.
* Setup a drafts folder on Dropbox so that I can write posts from anywhere.

## Ruby Setup

The first step is to install [Ruby](http://www.ruby-lang.org/en/). If you want to, you can compile this from the source but the better option is to use a third party tool to install and manage Ruby. The one I used is [RVM](https://rvm.beginrescueend.com/rvm/install/). 

By default OS X Lion comes with 1.8.7 pre-installed but Octopress needs 1.9.2. (I tried 1.9.3 at first but Octopress reported an error). I read on Stack Overflow that it's not recommended to upgrade the default installation but instead to install the newer version alongside it. RVM is designed to manage multiple versions of Ruby on a system so it's a nice fit here.

The command to install Ruby is `rvm install 1.9.2`. However when I tried this I got errors complaining that the C compiler was LLVM based instead of gcc. To get around this I added the following flag `rvm install 1.9.2 --with-gcc=clang`. Now we can tell RVM to use version 1.9.2 with this command `rvm use 1.9.2`.

## Install Octopress

Installing Octopress is very easy once Ruby is installed. The instructions can be found at [Octopress Setup](http://octopress.org/docs/setup/). The install command is `rake install` and then to build your website run `rake generate`. Octopress takes whatever pages and posts it finds under the source folder and builds these to a static website. These need to have particular file names and contents, so Octopress provides helper commands to generate a new post or page. These are `rake new_page["page_title"]` and `rake new_post["post_title"]` respectively.

## Export from Wordpress
To export from Wordpress I simply used their export tool. This exported all the posts in their own html files. This would have been fine for Octopress but I wanted the whole site in Markdown, so I converted each post from html to markdown and added to the source/_posts folder.

## Host on Github

Once you have the Octopress blog created you need to host it somewhere. I choose Github as it provides for free hosting of static blogs. Also if I do release some iOS code later it will be on Github, so there's no harm in getting it set up now. 

This [site](http://code.dblock.org/octopress-setting-up-a-blog-and-contributing-to-an-existing-one) has a very good explanation of what is going on with Github. Basically we build our website locally to a folder named public. We create a repository on Github for our webpage. Octopress then uploads the contents of the public folder to the master branch on this repository whenever we run the `rake deploy` command. It is recommended that you upload the site source to a source branch on the repository. That way you can always regenerate the website even if you delete the local files.

## Register URL

I decided to register www.gerardcondon.com for this site as I didn't want to be tied to a Github url in the same way as I was tied to the Wordpress.com one. I didn't need hosting or anything special - just forwarding. I used [Namecheap][]. The price is really cheap for a .com address - only 6 or 7 dollars a year. 

There are instructions on [Github Pages](http://pages.github.com) on how to set up the forwarding. You need to setup an A record to redirect to www.github.com for your url without the www part, i.e. gerardcondon.com will redirect to www.github.com. Then you need to create a CNAME file in github and then setup a CNAME redirect for the www part of the url, i.e. www.gerardcondon.com redirects to gerardcondon.github.com. However I ran into some problems doing this on the namecheap site. When I tried to setup the CNAME I got the following errors

`Improper records were NOT saved!`
`There were issues with some records and they were either set to default or completely removed. Please review the issues below and update again appropriately.`
`INVALID_ADDR: 'gerardcondon.github.com/' should not be an IP/ URL for CNAME record. (host name: www)`

However I just connected to the online chat support and the person assigned to me fixed it up in a few minutes - I have to say they were really helpful. This DNS stuff takes a while to settle down - I think it was a day or so before it had propagated out. Since then I've had no trouble with it.

## Octopress tweaks

I've made a few tweaks to Octopress to suit my own tastes. 

* I changed the indented-lists setting to true in sass/custom/_layout.scss. Previously the numbers and bullets for lists were not in line with the rest of the post text but instead were placed in the left margin. 
* I reduced the size of the blockquotes in the sass/base/_typography.scss file, from 1.2 em down to 1 em to be the same size as normal text.
* I changed the solarized theme from dark to light in sass/base/_solarized.scss.

It's really easy to make changes to Octopress, there is a folder called sass with most of the style stuff. There are a few configuration files also which can be updated. One nice thing about checking all this stuff into github is that you can go back and see what style updates you made in previous commits.

## Add Draft folder and sync with Dropbox

At this stage Octopress was installed on my Macbook Air and working perfectly for me. One limitation of this setup was that I could only edit posts from that machine. I didn't mind only being able to regenerate the site from the Air as that's not a common task, but I wanted to be able to write posts from anywhere. I also wanted the ability to write draft posts and not have Octopress automatically publish those.

I found a technique for managing draft posts on [Frozen Bytes](http://www.i-m-code.com/blog/blog/2012/01/25/target-any-deployment-environment-in-octopress-with-ease/). This allows you to create a drafts folder and provides commands for moving posts between the draft and published state.

Using this, I created a folder called drafts in Dropbox. I then symlinked to this from inside the sources directory on the Air. All my drafts are now available in Dropbox and I can edit these anywhere. This is especially useful when editing from iOS as there are a number of markdown editors which can edit files on Dropbox.

## Summary

I think that's everything! It was fairly straightforward in retrospect to set up. The new setup works really well. I'm very happy with the look of Octopress and the editing workflow in markdown is much better than using the Wordpress editor. Looking forward to using it in future.

[namecheap]: http://www.namecheap.com