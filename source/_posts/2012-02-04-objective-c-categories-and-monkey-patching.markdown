--- 
layout: post
title: Objective C Categories and Monkey Patching
date: 2012-02-04 18:00
comments: true
categories: Development

---
One of my favorite features of Objective C are [categories](http://developer.apple.com/library/ios/#documentation/cocoa/conceptual/objectivec/chapters/occategories.html). Categories allow you to add your own functionality to existing classes without the need for inheritance. You don't even need the source code of the modified class. You are only allowed add extra methods or modify existing methods. Adding extra instance variables is not permitted. This feature is generally known as [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch) (what a great name!) in languages such as Ruby.

Creating a category is straightforward. The interface and implementation files look the same as normal interface and implementation files except the category name is placed in brackets after the name of the class we are extending. For example to create a category MyCategory on MyClass the interface file looks like
{%codeblock MyClass+MyCategory.h lang:objc %}
#import "MyClass.h"

@interface MyClass (MyCategory)
    // Declare Methods here
@end
{% endcodeblock %}
and the implementation like this
{%codeblock MyClass+MyCategory.m lang:objc %}
#import "MyClass+MyCategory.h"

@implementation MyClass (MyCategory)
    // Implement methods here
@end
{% endcodeblock %}

The standard naming convention for category files is ClassToExtend+Category.(h|m). So for the above we would have both MyClass+MyCategory.h and MyClass+MyCategory.m files. The header file needs to be imported in any file that uses the category. For common extensions we can place them in the precompiled header (pch) files.

This has a number of applications. It allows you to add methods to classes (or indeed to alter existing methods) instead of having to create utility classes comprised of static methods. Thus the code is more clearly associated with the class to which it belongs. It allows you to create object-oriented code that favours composition over inheritance.

To give a concrete example, for my current project I wanted to be able to shuffle an array. Instead of having to create a ShuffleableArray subclass or implement an ArrayUtils.shuffle helper method, I was able to add a shuffle method to the NSArray type via a category.

Not only does the class to which you add the category get the new method, but all its subclasses gain the method also. This can be very useful if you are extending a class high up in the hierarchy. For example, another use I made of categories was to add debug methods to the UIView class e.g. to print data about itself and its subviews. Given that lots of the UI widget classes extend UIView this means that everything on screen got my new methods. I find trace methods like these very useful during unit testing and debugging. I tend to prefer adding print code where possible rather than step through the debugger, and for that case the ability to add functionality to existing classes is invaluable.

Naturally like all programming language features categories can be greatly abused. Changing the behaviour of existing functions can break code which assumes the older behaviour. I'd imaging this would be especially so with system classes such as NSArray etc. However done right, they look like a very useful feature which I'm sure I'll use a great deal.
