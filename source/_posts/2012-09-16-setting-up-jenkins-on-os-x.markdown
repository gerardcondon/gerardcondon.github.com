---
layout: post
title: "Setting up Jenkins on OS X"
date: 2012-09-16 13:24
comments: true
categories: Development
keywords: Jenkins, Bitbucket, Altassian, SourceTree, Git, XCode
---

First post in a long time. I didn't really do much iOS development over the summer and am only starting to get back into it in the past few weeks. I have some draft posts on my dealings with both Core Data and UITableViews and once those features are complete I'll post them.

Along with coding, I've been looking at adding some build infrastructure for my projects. Having seen [Jenkins][] used at work, I wanted to get a CI system set up for my builds here at home. I want to use it to kick off a build, run the tests and package up the iPhone app. By using something like Jenkins I can automate my build and testing process and also have a record of previous builds.

## Jenkins Install

The first step was to install Jenkins. They provide a Mac installer - the only annoyance is that it triggers the new OS X Gatekeeper dialog blocking unsigned apps from installing. Then I loaded locahost:8080 in my browser to find that Jenkins wasn't loading and there was no error message telling me what went wrong. D'oh. The issue is that Jenkins requires Java to run but it isn't installed by default on OS X. To install simply type `java` into a terminal and a message box will appear prompting you to install Java.

Then I followed the user setup from Moritz Haarmann's [blog](http://momo.brauchtman.net/2011/11/12/howto-install-jenkins-on-os-x-and-make-it-build-mac-stuff/). I'm not sure if the jenkins account part is still necessary, as when I got to the stage about changing the user name in the plist file, it had already been set to Jenkins. The changing permissions on the Jenkins home folder was needed though. After that I was able to run ssh-keygen and generate some keys.

## Updating Git Workflow

I installed the Git and XCode plugins from the Manage Jenkins screen with no issues. I then created a simple project to test it out. I just added a git clone step. This failed for me with the following errors

	Failed to connect to repository : Command "git ls-remote -h ..../MyApp.git HEAD" returned status code 128:
	stdout: 
	stderr: fatal: '..../MyApp.git' does not appear to be a git repository
	fatal: The remote end hung up unexpectedly

The problem here is that the repository is on my Dropbox account (as described [here](http://www.gerardcondon.com/blog/2012/01/26/version-control-in-xcode-using-git-and-dropbox/)). This folder is not visible to the jenkins user. I didn't want to mess around with changing permissions on the Dropbox folder so I looked for an alternative solution. A colleague at work told me about [Bitbucket][] from Atlassian. This is similar to [Github][] but allows for private repositories. As luck would have it, I had already been using another Atlassian product [SourceTree][] to manage my git repositories. This tool works seamlessly with Bitbucket. Once I uploaded my ssh key to Bitbucket I could simply add another remote repository to my local repository and push to it, all from within SourceTree.

Now that I had a repository on Bitbucket, it was time to integrate that with Jenkins. This turned out to be fairly easy. I uploaded the ssh key for the jenkins account to Bitbucket. The admin page for the repository showed its ssh address so I added a Git build step to the project using that ssh address. When the project is built now, Jenkins will download the latest code from the repository first and then continue on.

## Jenkins Issues

While Jenkins is mostly working fine now, there were a few issues I've run into. The git checkout isn't working at all using the Git Plugin. Originally it did but then I started getting the following errors
	FATAL: Could not checkout null with start point 02dbc3e456d6aa6079543eeaa8361bdebe8fac9d
	hudson.plugins.git.GitException: Could not checkout null with start point 02dbc3e456d6aa6079543eeaa8361bdebe8fac9d
		at hudson.plugins.git.GitAPI.checkoutBranch(GitAPI.java:956)
		...

	Caused by: hudson.plugins.git.GitException: Command "git checkout -f 02dbc3e456d6aa6079543eeaa8361bdebe8fac9d" returned status code 128:
	stdout: 
	stderr: fatal: reference is not a tree: 02dbc3e456d6aa6079543eeaa8361bdebe8fac9d

I wasn't able to find a solution to this so I had to stop using the Git plugin and just add an `execute shell` build step to clone the Bitbucket git repository. As I add more projects to Jenkins, I'll check if I get the same issues there also.

## Build XCode Project

The [XCode plugin][] for Jenkins made it really easy to build once setup. You add an XCode build step and configure it as necessary. I chose to do a clean build each time. The project I tested this on was a static library so I didn't need an IPA built or any of the version number updates but they should be useful when building a proper app.

When I first tried to build I got an error saying `You have not agreed to the Xcode license agreements`. I needed to run `sudo xcodebuild -license` in the terminal to fix this.

In order to run the unit tests, the XCode plugin recommends using two build steps, one to build the app and the other to execute the tests. The reason for this is that the test step requires extra parameters i.e. path to the simulator SDK. If you're using the OCTest framework which comes with XCode then the plugin will listen to the test output and generate JUnit compatible xml files. Jenkins can read these and add the to the build results page.

## Set up Coverage
Setting up code coverage for iOS projects is not an easy task. My gcov setup for XCode 4.3 still worked for me but the problem was to integrate this with Jenkins. Coverstory would not be of use here but I found this [gcovr][] script from the [Octo Talks] blog. Gcovr will convert the gcov output into a [cobertura][] xml file which can be read by Jenkins.

One problem I had was that for some reason, gcov wouldn't work on the files in the Jenkins workspace. I was getting `gcno:cannot open graph file`errors. Coverstory also didn't work on the files in this location. What worked for me was to copy the gcda and gcno files out to a temp folder under /Users/Shared/Jenkins/Home. Passing the original build location as the root parameter to gcovr allowed the cobertura plugin to find the source code in the html report, while also removing system headers from the coverage report. Here is the script.

	cp -r ${WORKSPACE}/build/${JOB_NAME}.build/Coverage-iphonesimulator/${JOB_NAME}.build/Objects-normal/i386/*.gc* /Users/shared/Jenkins/tmp/${JOB_NAME}
	cd /Users/shared/Jenkins/tmp/${JOB_NAME}
	gcovr -r $WORKSPACE/$JOB_NAME -x > ${WORKSPACE}/coverage.xml

The [Octo Talks] blog also suggested a number of other metrics which could be captured in Jenkins. I added the SLOC Count and the Clang Build Scan. Both of these required installing tools and then Jenkins plugins which used those tools. This instructions on the blog covered this. Ones that I've put on the someday/maybe list are the PMD plugin and OCLint.

At the end of all this, Jenkins is working quite well for my simple test project. Next step is to try it out on my app.

[Jenkins]: http://jenkins-ci.org/
[Bitbucket]: https://bitbucket.org/
[Github]: http://www.github.com
[SourceTree]: http://www.sourcetreeapp.com/
[XCode Plugin]: http://www.XXX.com
[gcovr]: https://software.sandia.gov/trac/fast/wiki/gcovr
[cobertura]: http://cobertura.sourceforge.net/
[Octo Talks]: http://blog.octo.com/en/jenkins-quality-dashboard-ios-development/
