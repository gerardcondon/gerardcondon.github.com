--- 
layout: post
title: Version Control in Xcode using Git and Dropbox
date: 2012-01-26 18:00
comments: true
categories: Development
keywords: git, dropbox, version control, setup repository, Xcode, iOS Development
---
When I first created projects in Xcode, I just let it take care of the version control for me by creating a Git local repository for my code. This was working fine for me but I could see that in future I would need more flexibility. I think it’s better to build that (and good programming habits in general) in at the start of a project when the codebase is small rather than refactoring down the line when it becomes a bigger problem.

One issue is that the local Git repository could only be used by one computer. If I wanted to share this code I would need to switch to using remote Git repositories. I also wanted to incorporate some form of remote backup for additional security.

[Github](http://www.github.com) (recommended to me by [Ian Hennessy](http://au.linkedin.com/pub/ian-hennessy/5/1b4/987)) would do the job just fine but the free accounts are limited to public repositories. If I was working with other developers then it would make sense to pay for one of their plans. The collaboration tools e.g. issue tracking, code review look really good. It’s certainly a site I can see myself using in the future.

However for someone like me who is a solo developer, these features don’t add much value. I really just needed the backup functionality. So for this, I looked at storing the repository on [Dropbox](http://www.dropbox.com). It’s private and the free two gigabytes should be more than enough to host my code. Also Dropbox is available on pretty much everything and works well with firewalls. As for backups, it’s bullet proof. I’ve had my Windows PC hard drive fail recently. In the past this would have been a nightmare to deal with. Now I just installed a new drive, reinstalled Dropbox and saw all my documents reappear.

One issue that I think may arise would occur if Dropbox has conflicting edits on the same file from multiple locations. In that case I’d imagine it’s merge resolution would mess up the Git repository. Fortunately I don’t think that this will apply in this situation. I will be the only person with access to the repository and I won’t be editing the same files from two PCs at the same time.

To set up Dropbox I followed the steps on [this](http://www.cimgf.com/2008/06/03/version-control-makes-you-a-better-programmer/) post from cimgf.com. This [answer](http://stackoverflow.com/q/1960799/1131820) on StackOverflow is also a good reference.

*   I created the project I wanted in Xcode and allowed it to set up a local Git Repository for me.
*   I cloned the repository using the bare keyword to a Dropbox location.
`git clone –-bare . ~/Dropbox/development/repositories/GCUtils/GCUtils.git`
The bare option is used as this will just clone the contents of the local .git directory to Dropbox. We don’t want to create a working copy of the code on Dropbox, as this may lead to Dropbox edit conflicts if that working copy is edited directly. In order to edit the code, a user should have to clone the repository to their machine, work on it locally and then push the changes to the Dropbox repository.
*   I created a remote alias.
`git remote add dbGCUtils ~/Dropbox/development/repositories/GCUtils/GCUtils.git`
*   I restarted Xcode and in the repositories tab of the Organizer, it picked up the remote repository automatically.

And that’s pretty much it for the initial setup. From here Xcode can manage the commits to the local repository and also the push and pull from the remote repository. The Xcode user guide describes how Xcode can interact with Git [here](http://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/Xcode4UserGuide/SCM/SCM.html).
