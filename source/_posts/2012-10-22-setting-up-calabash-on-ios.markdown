---
layout: post
title: "Setting up Calabash on iOS"
date: 2012-10-22 23:38
comments: true
categories: 
keywords: Calabash, Cucumber, iOS, Automated Testing
---

I've been implementing some new features in iOS and in the process refactoring some existing code. As I've been making these changes, I began to feel that the code was reaching a tipping point where I was as likely to break existing features as successfully add the new functionality. 
In particular I was experimenting with table view controllers and there are a lot of functions to override depending on what behaviour you want. I was finding that changes for one type of behaviour were impacting functionality that I thought I had solved previously. I hadn't been doing much automated testing up to now apart from some unit testing, so I was worried about adding regression bugs.

I decided to take a break from coding in order to implement some proper tests and specifically I wanted to do some UI testing. For this I chose the Calabash framework. This is developed by the people at [Less Painful][]. One of them, Karl Krukow, has a detailed [post](http://blog.lesspainful.com/2012/03/07/Calabash-iOS/) comparing Calabash to the other automated frameworks out there.

# Cucumber

Calabash is based on a Ruby test framework called [Cucumber][]. Cucumber allows for tests to be written in high level language called Gherkin. Gherkin is written in natural language and so can be understood by anyone - not just developers. It acts as documentation as well as a test framework. I could see this as being very useful when writing an application for a client. By giving them the Cucumber tests, they can see in plain English what is being implemented and should be able to give constructive feedback. Also the tests are automated and are run from the command line.

While learning Cucumber the two main resources I used were

*   The [Cucumber][] website which has a number of tutorials and a great wiki.
*   The Pragmatic Programmers' Cucumber [book][PragProg]. This is an excellent book. I highly recommend reading this before implementing any tests. 

# Installation

Cucumber requires Ruby to be installed on your system (I had previously installed it while setting up Octopress). The Calabash install process is documented on their Github [page](https://github.com/calabash/calabash-ios). I used the Fast Track installer using these steps.

*	`gem install calabash-cucumber`
*	`calabash-ios setup`
*	`calabash-ios sim acc`
*	`calabash-ios gen`

This created initial test files which you can run from the command line using `cucumber`.

One thing that worries me about the install process is that it creates a new scheme specifically for Calabash use. I don't really like this solution as it breaks DRY and requires keeping the original scheme and the new Calabash scheme in sync. From my experience in programming, anything that is required to be kept manually in sync, won't be. I would have preferred for the Calabash libraries to only be included in the Debug target of the main scheme or for a new target to be created on that scheme.

A good blog on the Calabash install and initial test setup is [here](http://www.moncefbelyamani.com/ios-automated-testing-with-calabash-cucumber-ruby/).

# First Steps with Calabash

* I think the Calabash [wiki][] is the best starting point for Calabash.

* The first thing I tried was the console using `calabash-ios console`. I recommend this to play around and see what objects are visible to Calabash.

* The command `query("view")` shows everything on screen. We can isolate specific views by filtering on the accessibility label. In some cases this wasn't useful for me. For example, I have a table where each cell has a text field. Each of these has the same accessibility label so instead I filtered using the :text property.

* We can find out what accessibility labels are visible using `label "view"`. Once we have a label we can do things like touching or swiping on the view or checking that the view exists using `view_with_mark_exists()`.

* The accessibility inspector can be used to identify the classes of UI elements and the on-screen hierarchy. Launch this from XCode under the menu item XCode->Open Developer Tool->Accessibility Inspector.

* I found that the built-in Calabash [steps](https://github.com/calabash/calabash-ios/blob/master/calabash-cucumber/features/step_definitions/calabash_steps.rb) were very useful to base mine on.

* A problem I had was querying for the currently active textfield from a table of textfields. The issue was that all those textfields had the same placeholder text. To get the correct one I filtered by `isUserInteractionEnabled`.

* Errors in the name of the selector passed to the `backdoor` command show up as connection errors.

* Initially I struggled with writing the tests at the correct level. The temptation is to be really specific in terms of UI elements e.g. When I touch X button and swipe on Y label. However you really need to describe them at a higher level e.g. when I add a new contact, when I delete an appointment etc.

* Initially I was unable to run a backdoor command in the before hook, which runs before each test. I had wanted to reset my Core Data database & UI at this time. The reason here is that the Calabash framework itself uses before hooks to connect to the app and I think my hook was being called before theirs. To solve this Calabash added support for defining an `on_launch` function which is called after the simulator has started. To implement use the following template in the `hooks.rb` file.  

		class CallbackWorld
		 include Calabash::Cucumber::Operations
		 def on_launch
		   # here I can call backdoor and reset the app state
		 end
		end

		World do
		 CallbackWorld.new
		end

* My typical test layout is to use 
	* **Given** to put the system in a specific state - this is where I use the backdoor function to set up the app model. I do things like erase all the Core Data objects here (from [Stack Overflow](http://stackoverflow.com/questions/1077810/delete-reset-all-entries-in-core-data)) to ensure that each test starts with a clean slate.
	* **When** to perform an action. These are driven through the app UI.
	* **Then** to check the results of the actions. I've typically done these using the app UI e.g. checking for the existence of views, the state of the UI etc. As an alternative I could also use the backdoor function to check the model here.

# Issues with Calabash.

It wasn't all smooth sailing as I did run into a few issues along the way.

* I wasn't able to get it to integrate with my installation of Jenkins, which is unfortunate as Cucumber can output in JUnit format so it's a perfect fit for Jenkins. The problem here is on my side as lots of users on the Calabash Google Group are running Calabash from their CI system. I think it due to the jenkins user not being able to launch the iOS simulator. I need to investigate further on this.

* I had found that Calabash would frequently drop the connection to the app and fail the tests as a result. Once a test run started seeing these errors for a test (`Unable to make connection to Calabash Server at http://localhost:37265/`) then each following test in the run would show the same error. Rerunning the tests would normally sort it out in the next run or two. Both the app and Calabash would be running fine but just couldn't seem to connect to each other. This unreliability was the most disappointing part of the tests for me.  
	
	**Update:** Karl pointed me to the Google Group and I saw some [updates](https://groups.google.com/forum/?fromgroups=#!topic/calabash-ios/NdExaULsHz4) that were needed to work with the new iOS 6 Simulator. I had seen that black screen issue as part of trying to get it working with Jenkins. So I've updated to the latest version of Calabash and on the first test run, everything worked fine. Hopefully this will resolve the issue in future.

# Initial Impressions & Next Steps

I have to say that I'm very impressed with Calabash and indeed Cucumber in general. I think that writing these type of tests beforehand would be helpful for requirements gathering and feature design. The tests themselves are not only useful as tests but also as documentation. Unlike Word docs, we can be guaranteed that these accurately describe the current state of the system, given that they are actually run against it.

My plans for the future

* I need to add more tests. I initially tested this with just a single feature and that's worked out so well that I'll add this to the other features.
* From now on, I also plan to write these type of tests for a feature before implementing it. I think that the Cucumber tests especially with the language they are written in are very effective when thinking through the behaviour of the app. They force you to describe it in high level terms rather than just diving into coding.
* My tests currently have a lot of duplication. Once I have written a lot more of them and I gain more Ruby experience, I will look to refactor them.

**Update:** After my initial post Karl Krukow emailed me with some updates on the issues I was having and I've integrated those into the blog post. He pointed out that there is a [Google Group](https://groups.google.com/forum/?fromgroups#!forum/calabash-ios) for Calabash on iOS where you can ask questions and share information.

[Cucumber]: http://cukes.info
[Calabash]: http://calaba.sh
[Less Painful]: http://lesspainful.com
[PragProg]: http://pragprog.com/book/hwcuc/the-cucumber-book
[wiki]: https://github.com/calabash/calabash-ios/wiki/01-Getting-started-guide