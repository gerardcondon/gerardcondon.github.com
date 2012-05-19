---
layout: post
title: "Localizing iOS Apps"
date: 2012-05-19 16:54
comments: true
categories: 
keywords: iOS, Localization, Internationalization, Linguan, storyboards
---

I was looking at a tutorial for creating a settings bundle for my app and it had separate files for different languages. I realised that I hadn't a clue how to localize iOS applications. I decided I'd better look into it now, rather than at the end of development, in case it required me to change existing code. Much better to implement with localization in mind, than retrofit at the end of development.

I'm interested to see how many translations I'll be able to include in the app. I'm not sure how easy it will be to do the translation or even how much text there will be to translate. I'm planning a Chinese translation and for sentimental reasons I'll do an Irish translation. I'll probably try to get some European languages done also. I think that once I put in the infrastructure to do one language then adding additional ones shouldn't be too much bother.

The basics of implementing localization is that you externalize all strings in your application to .strings files and then provide versions of these files for each language you support. The process for creating the basic localization.strings file is outlined [here](http://userflex.wordpress.com/2011/10/20/localized-strings-xcode4/). In your code you can get the localized values for these strings using `NSLocalizedString(@"Externalized String Name", @"")`.

## Localization Tools

Using this method, we will have to deal with lots of .strings files. Xcode doesn't provide much help here so I looked around for third party apps. I bought [Linguan][] on the Mac App Store and it looks very good. You point it at an Xcode project file and it locates all the .strings files and presents them in a nice table. It can generate text files which you can send on to your translators for them to translate and re-import their translations. I'm very pleased with it so far.

Apart from having .strings files for strings in the code, I also need to localize the app's storyboards. Albert Mata has a fantastic tutorial on how to do this [here](http://www.albertmata.net/articles/introduction-to-internationalization-using-storyboards-on-ios-5.html). A big problem with localizing storyboards is that changes in one language's storyboards are not propagated to the others. Manually keeping these in sync would be a nightmare. Thankfully there is a handy script [here](http://code.google.com/p/edim-mobile/source/browse/trunk/ios/IncrementalLocalization/localize.py) by Ederson Machado, which will update the .strings files for the storyboard and also keep the storyboards in sync. There is a nice video outlining the features of the script [here](http://www.youtube.com/watch?v=cF1Rf02QvZQ). By adding this script as a build run script, we can ensure that our storyboards are always in sync.

## Localization Workflow

My localization workflow is as follows:

* First I created multiple storyboards for each language using the Localization area on the sidebar. This will create a language.proj folder for each language.
* I ran the localize.py script on the project. This generated the mainstoryboard.strings files in each of the language folders.
* I manually add these files to Xcode. First I added the English version. Then I clicked the plus button in the Localization sidebar and added the other languages. Xcode is able to find those .strings files and link them to the overall file.
* I create a localization.strings file to deal with the strings in the code and added all languages.
* I ran genstrings but I don't think this is needed if you're using Linguan.
* When I opened the project in Linguan it was able to find all the strings files, including those from the storyboard.

I'm satisfied now that I have the localization mostly under control. One thing I still need to look at is how does this affect testing. Hopefully any test framework that I used will be able to run the tests in all languages.

[Linguan]: http://www.cocoanetics.com/apps/linguan/