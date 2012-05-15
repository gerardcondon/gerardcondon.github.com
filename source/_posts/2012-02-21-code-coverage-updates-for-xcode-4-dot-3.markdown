---
layout: post
title: "Code Coverage Updates for Xcode 4.3"
date: 2012-02-21 00:02
comments: true
categories: Development
keywords: XCode 4.3 Code coverage, libprofile_rt, llvm, gcda
---
In a previous [post](http://www.gerardcondon.com/blog/2012/02/17/adding-code-coverage-to-unit-tests/), I described how I got code coverage up and running in Xcode 4.2. Apple have just released Xcode 4.3 and unfortunatly updating to this has broken my code coverage. The reason is that 4.3 removes the /Developer folder and moves this internally to the Xcode.app package. While this has good consequences - it should be possible to update Xcode from the App Store like a normal app - unfortunately it has also removed the libprofile_rt library from /Developer/usr/lib. I haven't been able to find a new version of this library in Xcode 4.3. When I tried using the version from 4.2, I got "mach-o but wrong architecture" errors.

This means that when building my unit tests with Coverage I get link errors saying that I am missing llvm_gcda functions (llvm_gcda_start_file, llvm_gcda_increment_indirect_counter, llvm_gcda_emit_function, llvm_gcda_emit_arcs). I searched on Google to try and find a solution for 4.3 but it seemed all solutions were for 4.2 so I needed to try to solve this myself. The solution I came up with is fairly hacky. I'm hoping that as more people upgrade to 4.3, a better solution will be found and I can switch to that.

My solution is to take the actual file which contained the gcda functions in libprofile_rt, add it to my own source code and compile it myself. The file in question can be found on llvm.org - [here](https://llvm.org/svn/llvm-project/llvm/trunk/runtime/libprofile/GCDAProfiling.c). To get it to build locally I deleted the win32 and sys includes (lines 27 to 31 inclusive) and replaced the llvm include (line 23) with `#include "stdint.h"`. This builds fine using the Coverage build configuration and outputs the same gcda and gcno files as before. To make sure that this code doesn't end up in the released project by accident, I've included it in the unit test bundle rather than the app bundle. This is fine for me as I only need coverage when running the tests anyway.

__Update:__ Some feedback from Tom Black, who emailed me to point out that libprofile_rt can be found in `/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/` 
However this library did not work for either of us. Instead we got errors like 
`The bundle “GCUtilsTests.octest” couldn’t be loaded because it is damaged or missing necessary resources.`
`Library not loaded: @executable_path ... libprofile_rt.dylib`
`Reason: image not found`  
Also this library didn't produce .gcda files but instead generated .dia files which didn't work in Coverflow.

What worked for Tom, but not for me, was not to include libprofile at all. This removed these errors and another incompatible binary error. Now however he got the following error. 
`Detected an attempt to call a symbol in system libraries that is not present on the iPhone:
fopen$UNIX2003 called from function llvm_gcda_start_file`
He found the solution to this issue [here](http://www.infinite-loop.dk/blog/2012/02/code-coverage-and-fopen-unix2003-problems/). I still got link errors when I followed this though so I need to stick with my original solution.

__Update:__ Another commenter Rajiv emailed me to say that creating a new target for his project made the coverage work for him. It generated the .gcda and .gcno files when he exited using exit(0). I've tried creating a new simple iOS app and I don't get any errors for this when I enable coverage.

One thing which might explain why it still doesn't work for some of my projects is the type of project being built. My original project was a static library. I tried to create a new static library project and when I try to enable coverage I get the same link errors as above.


