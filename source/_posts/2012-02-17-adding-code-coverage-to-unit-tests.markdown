---
layout: post
title: "Adding Code Coverage to Unit Tests for Xcode 4.2"
date: 2012-02-17 22:27
comments: true
categories: Development
keywords: Xcode, Code Coverage, 4.2, iOS
---
If you are going to the effort of having unit tests, then you really need to have code coverage. This will allow you to ensure that your tests are exercising all your code. It is really easy to spot a missing test when you see a branch of an if-else with no coverage. Code coverage for me is also a way to motivate myself to implement the tests. It's like high scores in games - it gives me a target to aim for. Coverage gives feedback that your tests are actually doing something, and with each test you add, you can see the areas of untested code steadily decrease.

To set up code coverage in Xcode 4.2 I used the guide on these blogs - [Infinite Loop](http://www.infinite-loop.dk/blog/2011/05/covering-it-all-up/) and [Matt Rajca](http://mattrajca.com/post/8749868513/llvm-code-coverage-and-xcode-4). Originally only the gcc compiler was supported but since then the Clang compiler has had code coverage enabled. This is important as we need to use Clang for ARC and static analysis among other things. 

### XCode Build Settings

Xcode has the concept of build configurations which can have built settings altered independently of each other. The two default configurations are Debug and Release. We want to add a new configuration named Coverage. To do this duplicate the Debug configuration. 

{% img /images/codeCoverage/NewBuildConfig.png New Build Configuration %}

In order to specify different settings for configurations in Xcode, hover over the left hand margin directly to the left of the setting we wish to update. An arrow will appear which when clicked will expand to an additional three lines for Debug, Coverage and Release. Editing the top line will alter the setting for all versions. Altering either of the new lines will only update the setting for that version of the build.

{% img /images/codeCoverage/BuildSettings.png Build Settings %}

*	Open the Build Settings tab for the main target. Under "Apple LLVM 3.0 compiler - Code Generation", enable both
    *	"Generate Test Coverage Files"
	*    "Instrument Program Flow"
*    We now need to add the libprofile_rt library to the build. This will link in the implementations of the coverage functions. If you are seeing link errors like "_llvm_gcda_increment_indirect_counter", referenced from:", then you have not done this step.  
    This library is located in /Developer/usr/lib. This folder contains libprofile.a which is a static library and a libprofile.dylib which is a dynamic linked library (similar to a .dll in Windows). To link this open the Build Phases tab for the main target. Expand the "Link Binary with Libraries" and click the + button. Click "Add Other" and then find libprofile_rt.dylib in the file viewer and add it.

### Executing Tests

Now your unit tests should run and if you examine the build output files under the DerivedData folder you should find .gcda and .gcno coverage files along with your .o files. To find this folder you can go to the Projects tab in the Organizer and select your project. The path to the Derived Data folder is listed here along with an arrow which will open the folder in the Finder. 

{% img /images/codeCoverage/DerivedData.png Derived Data %}

From here the path to the coverage files is Build/Intermediates/${Project Target}.build/Coverage-iphonesimulator/${Project Target}.build/Objects-normal/i386. Replace ${Project Target} with the name of your project target.

### Viewing Coverage Data

To view the coverage data I used [CoverStory](http://code.google.com/p/coverstory/). Simply open the app and point it at the coverage folder. This will give a nice two pane display showing your files on the left and an editor which shows how often each line of code was hit. Now you can identify untested code and add new tests to cover them.
