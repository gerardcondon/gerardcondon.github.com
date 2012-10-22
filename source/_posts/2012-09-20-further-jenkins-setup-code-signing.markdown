---
layout: post
title: "Further Jenkins Setup for iOS"
date: 2012-09-20 01:15
comments: true
categories: 
keywords: XCode 4.5, jenkins, iOS, build, app
---

Last [time](http://www.gerardcondon.com/blog/2012/09/16/setting-up-jenkins-on-os-x/) I went through how I set up jenkins on iOS and built a simple static library. All in all, that process went ok. The next step was to build an actual app. Unfortunately I encountered a lot more issues with this.

The first error I got was 

    Code Sign error: The identity 'iPhone Developer' doesn't match any valid certificate/private key pair in the default keychain` 

This [post](
http://stackoverflow.com/questions/9245149/jenkins-on-os-x-xcodebuild-gives-code-sign-error/9246321#9246321) on Stack Overflow has a really detailed set of instructions on how best to fix this. Unfortunately as my iPhone developer account is not a business one, I can't add a jenkins user to it. So to solve my problem I added my certificate to the system keychain so that now my local jenkins user can access it.

Once that was out of the way, the next issue to rear its head was a provisioning profile error. 

    Code Sign error: A valid provisioning profile matching the application's Identifier '...' could not be found

I found the solution to this again on [Stack Overflow](http://stackoverflow.com/questions/10454628/xcodebuild-code-sign-error-provisioning-profile-x-cant-be-found).
I needed to copy my provisioning profile from `~/Library/MobileDevice/Provisioning` to the jenkins user's folder at `/Users/Shared/Jenkins/Home/Library/MobileDevice/Provisioning Profiles`.

The next reason for the build failing was that I was invoking xcodebuild using a target instead of a scheme. The target worked fine for the static library but was failing for the app as it was unable to find all the include files. Once I switched to building a scheme, the build worked fine. Building via a scheme or target is easy to configure using the Jenkins XCode plugin. You set the XCode Schema File entry to the name of the XCode build scheme.

Nearly there! Now that the build was working I turned my attention to getting the unit tests running. The `TEST_AFTER_BUILD` flag that had worked before wasn't working this time. Fortunately I found the solution on Peter Jihoon Kim's [blog](http://www.raingrove.com/2012/03/28/running-ocunit-and-specta-tests-from-command-line.html). I needed to create a new scheme to run my tests. Once that was done I got this error

    /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Tools/Tools/RunPlatformUnitTests:95: warning: Skipping tests; the iPhoneSimulator platform does not currently support application-hosted tests (TEST_HOST set).

Peter's blog describes the fix for this. You need to patch the `RunPlatformUnitTests` file in order to remove the warning printout and invoke the tests.

The last error I had to deal with was with the code coverage. I started seeing errors with `fopen$UNIX2003` and `fwrite$UNIX2003`. I had seen this issue on other blogs before but hadn't encountered it myself until I tried building an app using the command line. The solution as described [here](
http://stackoverflow.com/questions/8732393/code-coverage-with-xcode-4-2-missing-files) is to add implementations of these into your application which simply call the standard C functions.

    #include <stdio.h>

    FILE *fopen$UNIX2003( const char *filename, const char *mode )
    {
        return fopen(filename, mode);
    }

    size_t fwrite$UNIX2003( const void *a, size_t b, size_t c, FILE *d )
    {
        return fwrite(a, b, c, d);
    }

One unresolved issue was that I couldn't get clang scan plugin working for this setup. I think the issue here is that it was using targets instead of schemes. I left this one go as I didn't feel it important enough to get working. XCode runs this while I'm developing anyway. I may come back to it in future once everything else is stable.

### XCode 4.5 Updates

So, just when I got everything working for XCode 4.4, along comes XCode 4.5. Ever since the 4.2 to 4.3 transition broke code coverage for me, I've been a bit wary of XCode updates. Sure enough after installing 4.5 and trying to build my app, I got an error message saying that the calabash framework was missing a armv7s slice
	ld: file is universal (2 slices) but does not contain a(n) armv7s slice: 
	...calabash.framework/calabash for architecture armv7s
[Calabash][] is part of an iOS testing framework based on [Cucumber][]. I've just started to implement this form of testing and it looks really nice. Getting back to the build issue, Ash Furrow has a detailed [post](http://ashfurrow.com/fix-ios-6-sdk-linker-error) on this issue. We need to remove armv7s (which refers to the new A6 chip in the iPhone 5) from our valid architectures. Once Calabash gets updated to support this architecture, I can reenable it.

Once everything built in XCode, it was time to try the jenkins build. Naturally this broke also. The problem was that the XCode plugin couldn't find the 5.1 simulator. 

    xcodebuild: error: SDK "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator5.1.sdk" cannot be located.

XCode 4.5 had wiped out the simulators for 5.1 so I needed to redownload them. I was able to download these from Preferences->Downloads->Components. XCode also prompted me to download the 6.0 simulator so I got that out of the way while installing 5.1.

Once the simulators were installed I started hitting the same `RunPlatformUnitTests` error as above. When I opened the script file to reinsert Peter's edits I saw that it had changed slightly. Now the error printout was happening in a function on line 80 which overrode `RunTestsForApplication()`. I deleted that function and I added back in the original patch at line 118 so that now it looks like
	
	if [ "${TEST_HOST}" != "" ]; then
        # All applications are tested the same way, by injecting a bundle.
        # The bundle needs to configure and run the tests itself somehow.
        export CFFIXED_USER_HOME="${BUILT_PRODUCTS_DIR}/UserHome/"
        mkdir -p "${CFFIXED_USER_HOME}"
        mkdir -p "${CFFIXED_USER_HOME}/Library/Caches"
        mkdir "${CFFIXED_USER_HOME}/Library/Preferences"
        mkdir "${CFFIXED_USER_HOME}/Documents"
        export OTHER_TEST_FLAGS="${OTHER_TEST_FLAGS} -RegisterForSystemEvents"
    
        RunTestsForApplication "${TEST_HOST}" "${TEST_BUNDLE_PATH}"
    else
        # If no TEST_HOST is specified, assume we're running the test bundle.
        
        RunTestsForBundle "${TEST_BUNDLE_PATH}"
    fi

These changes fixed the 4.5 build issues and now I have a working build and test setup again. Finally!!

[Cucumber]: http://cukes.info
[Calabash]: http://calaba.sh