---
layout: post
title: "Managing Sub-Modules with Git Subtree"
date: 2012-11-05 23:30
comments: true
categories: 
keywords: Git, subtree, submodules, external repositories
---

I've been using Git for a while now, both at work and also for my iOS projects. I'm really liking it and have nearly sorted out my workflow with it. One problem that remains for me though is how to manage external dependencies. If I need to include a library from Github for example, how do I integrate it with my source. 

The main issues that I want to solve are

*	What happens if the external library disappears? This is especially relevant for online sources like Github. Given the brittle nature of the web, I think we can take it as a given that a lot of these Github repositories will be deleted over the coming years. In this scenario I need to have a local copy of the code as a backup.

*	I should be able to update the local code with the latest or a specific tag from the external repository.

*	I want to be able to create common libraries for my own code which I can share between applications.

*	It may be that I will need to modify the code, so how do I maintain those changes. Perhaps I will need to push those changes to the external repository. This seems likely for my own common libraries.

*	I want to be able to check out all the code needed to build an application from the source control using a single checkout/clone command. This means that each application will have a full copy of all the code that is needed to build it.

One solution that I've seen used in other projects is to use git submodule links. A git submodule points to a version of an external repository. The ones I've seen for iOS libraries will usually point to Github. When you run the `git submodule init` command, git will follow these links and download the source into your project. 

However in researching this, other people have reported problems using git submodules. They don't seem as simple and as usable as I would need. I'm seeing issues on [sites](http://codingkilledthecat.wordpress.com/2012/04/28/why-your-company-shouldnt-use-git-submodules/) regarding detached heads, branching, different versions etc. I just know that I will hit all these issues and probably a few more besides!

In looking for an alternative I came across git subtrees on Wolf Rentzsch's [site](http://rentzsch.tumblr.com/post/22061209807/apps-i-love-git-subtree). This seems to fit all my needs and appears less error prone than git submodules. The code can be downloaded [here](https://github.com/rentzsch/git-subtree) and here's a good [tutorial](http://psionides.eu/2010/02/04/sharing-code-between-projects-with-git-subtree/) on using subtrees.

My basic usage of it, i.e. to integrate a library from Github would be something like 
`git subtree add --prefix=GCUtils/External/CocoaLumberjack --squash https://github.com/robbiehanson/CocoaLumberjack.git master`
I use the squash option to compress all the commit history of the library to just one commit message in my repository.

One issue I ran into was adding a project which itself contained submodules. In this situation I search and find all the .gitmodules files and add those manually as subtrees in my repository.