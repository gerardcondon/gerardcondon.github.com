---
layout: post
title: "Working With Typescript"
date: 2014-09-19 01:24
comments: true
categories: Development
keywords: TypeScript, JavaScript
---

For the past year and a half, my team at work have been using [TypeScript][] to implement a large single page application in [Backbone][]. We're over three quarters of the way through the project, closing in our our first release and here are my thoughts on using TypeScript to date.

This is my first major foray into web development, so it also required ramping up on HTML, CSS and REST. Previously I had used JavaScript to implement the client side of a websocket API, but that project was non-GUI work.

## Reasons for choosing TypeScript

The company has settled on TypeScript for web programming due to a number of factors.

* Firstly is the additional security/peace of mind provided by type checking. For example, this prevents a lot of mistakes in calling functions with the wrong parameters. It makes some refactorings easier, as the compiler can tell you when you're calling functions that no longer exist or are passing the wrong types to a function.

* TypeScript adds classical Object-Oriented constructs to JavaScript e.g. interfaces, classes with inheritance. Rather than having to chose a library to implement inheritance, it is instead a first class language feature. I find this, along with having a proper super keyword to be much more usable in practice than prototypical inheritance. 

  A nice feature is that TypeScript has support for implicit interfaces. The compiler will figure out if a class implements an interface rather than requiring that the class declares a list of implements X clauses in its definition. This reduces the friction of dealing with the type system.

* TypeScript is compatible with JavaScript so any library out there can be used with our code with no problems.

* Better tooling. The idea behind this was that, given that TypeScript has a proper type system, this would allow better tooling such as Intellisense. The theory was that programming in TypeScript would be a better experience because the IDE would be better.

  As an aside, I would question the value of Intellisense and the type of code it leads to. Take Java for example. When you combine Intellisense with modern IDEs' ability to automatically import files, you greatly lower the barrier to coupling. It is no problem to include remote files, grab the inevitable Singleton instance, and execute large Law of Demeter busting method chains on them. 

        GlobalSingletonReference.getInstance().getSomethingElse().andItsChild()
            .lawOfWhatExactlyNow().pleaseStopSoon().noTheresMore().invoke()

  I think Java tools have given the ability to create larger programs than can be properly maintained.

These features were seen as key to creating a more maintainable source code base especially at large scale.

## My Experience with TypeScript

We had written a JavaScript prototype in Backbone and we ported that to TypeScript, so that we could compare and see how it went. I tried to use TypeScript as much as possible to be fair to the experiment. You can get away with basically writing JavaScript and passing it through the TypeScript compiler but that's no good to anyone really.

I found it made my code look more like Java or C#. This was especially the case with class definitions. Defining a class hierarchy in JavaScript is terrible - needing to set the prototype to the parent's prototype, manually defining super etc. The TypeScript version is very familiar to a Java or C# coder. Our group of TypeScript programmers were converted Java/C++ programmers so this was a huge bonus.

Having interfaces was great. They're very useful in defining APIs and especially so for documenting external APIs. One thing I hate about JavaScript, is having to read documentation or readme files for third party libraries in order to find out their API. An interface definition in the language itself is far superior, as it is a lot more concise and guaranteed to be correct, having gone through the compiler.

In the end, the code had the same classes with the same class names but the class implementations were far more readable due to the OO nature of TypeScript and the ability to define and program to interfaces.

I did find that refactoring was easier - operations like adding additional parameters to functions were trivial compared to JavaScript. For the JavaScript code, I had to rely on my unit tests to assure me that my refactorings were correct but here I could offload a lot of those tests to the compiler.

When we started on TypeScript it was version 0.8. The compiler was a bit rough then and crashed on some invalid input rather than reporting errors. It has been steadily improved since then and version 1.0 is perfectly fine for us, reporting the correct errors for all the previously crashing cases. Also the language has been added to and improved over time.

## Things I didn't like about TypeScript

On the flip side there are a lot of things that I don't really like about TypeScript.
Some features of JavaScript e.g. different return types, can't be represented in TypeScript. In these circumstances you find yourself using the "any" type - the equivalent of using Object in Java. The problem with this is that using it completely circumvents the type checker. Thus the more complex code ends up being the code with less type checking. 

Other JavaScript features such as mixins have such horrible syntax in TypeScript (see [here](https://typescript.codeplex.com/wikipage?title=Mixins%20in%20TypeScript)) that they're basically unusable. Mixins in particular require repeated boilerplate code to get past the compiler. That was a pattern with a lot of the issues I had with TypeScript. As you try to do the more dynamic JavaScript stuff, you end up writing and repeating declarations to get the compiler off your back. Ideally there would be some way to say to the TypeScript compiler that we are going to implement this interface dynamically - the implementation may not be here now but it will be at runtime. We ended up generating a lot of this boilerplate code using Ruby and Erb (a topic for another blog post).

I tried debugging with source maps in Chrome but I wasn't a fan of the experience. Breakpoints would always get shifted a few lines and it was hard to get them to break on a function. I was constantly wondering if I had the correct version rather than a cached sourcemap and did the TypeScript match up to the JavaScript. I ended up just using the compiled JavaScript for debugging.

Continuing with the last point, with some TypeScript features, you need to know what type of code was generated e.g. did a variable assignment get generated in the constructor or on the prototype. For example, this is necessary when integrating with Backbone. Instance variables in TypeScript are not defined on the prototype but instead in the constructor after the call to super. This means that they are not defined by the time the Backbone constructor is called. The Microsoft [solution](https://github.com/Microsoft/TypeScriptSamples/blob/master/todomvc/js/todos.ts) is to put the call to super in the middle of the constructor but this looks wrong to any Java programmer and I could see them inadvertently breaking the code by moving super to the top of the constructor. 

TypeScript's support for Generics was almost good but again there are some issues. The main one I ran into is that you can't create a new instance of the generic type e.g. for a generic type T you can't do `var x = new T()`. There are ways around this by passing in functions that create objects but the code they lead to is fairly bad.

The idea that types would lead to better tooling didn't pan out for us. Taking IDEs first, I think the main tool that supports TypeScript is [Visual Studio][]. There is also JetBrains' [WebStorm][]. As IDEs go these seem perfectly fine. It's a bit hard for me to evaluate this as I'm not a fan of large IDEs. One issue with these is that, especially in the case of Visual Studio, they require large license fees. I don't like criticising tools on cost issues, as I feel that companies should treat these as a required cost of hiring programmers. Unfortunately, a lot of companies don't, so if I'm required to buy a personal license, I much prefer to buy a license for a tool like [Sublime Text][]. 

TypeScript files import other files by means of a reference path at the top of the file. This is almost like Java except unfortunately the compiler does not enforce these, thus requiring them to be manually maintained. This is impossible to get right for a large project. The only essential ones are those for your base classes but if you leave out the others then IDEs have problems locating type declarations. If you have extraneous references that are not technically needed then this can lead to the TypeScript compiler generating invalid code that defines subclasses before their parent classes. When run, these cause runtime exceptions. Not a great situation.

There aren't a great number of TypeScript plugins for Sublime Text and there is no official one from Microsoft. Also there are no code quality tools such as linters. It's not much use running the JavaScript versions as the only thing they can run on is the compiled code. The set of tools available for JavaScript is much larger and more mature. Even where you would think that having types would allow for newer tools e.g. static analysis or dependency graph generators, there is nothing.

From a language point of view I wonder if trying to make all valid JavaScript code be valid TypeScript code is harming them? Would they be better going for a more C# like language and mandating that any JavaScript should be in separate files? That's what we ended up doing anyway - we didn't want to mix our JavaScript and TypeScript codebase.

### Integration with Third Party Code.

In order to use external JavaScript files in TypeScript, you must first create a definition file for the JavaScript API. This declares, in a manner similar to Java interfaces, the functions, classes and interfaces that the JavaScript code exposes. These files can be a pain to locate and maintain. There is a Github repository [Definitely Typed][] which maintains a collection of .d.ts files for popular JavaScript libraries. These are typically of a high standard but we have had to add missing functions to some Backbone .d.ts files. If there is none online you have to write one yourself which can involve reverse engineering the API and types of the library.

I think there is a large risk in using these, given that they are neither maintained by the library owners in question or by Microsoft themselves. It is problematic to update the libraries as now you have to also update the .d.ts. files. Everything going well, the Definitely Typed version will be updated to the latest version but there are no guarantees. What happens if the maintainer of this repository gets fed up and stops updating the files?

However once they are found, these .d.ts files can be extremely useful. When working with websockets, the TypeScript lib.d.ts file was the best documentation I found on the subject. I think the interface/protocol concept is a great addition to any programming language. It is especially useful for documenting APIs and it harms Ruby and JavaScript not to have such a construct.

It can be a bit tricky to integrate your TypeScript code with existing JavaScript libraries. As outlined above with Backbone, some libraries need to have code that's on the prototype so you need to know the code that TypeScript generates. Also where in the hierarchy does the library go? We found it best to have the JavaScript classes at the top of the inheritance tree and TypeScript in the subclasses.

## Conclusions

One issue I'd have with TypeScript is trying to gauge Microsoft's commitment to the language. Are they really in this for the long term? For example, the code samples on their website haven't been updated in ages. Also how large can the TypeScript community get? Are there really going to be a critical mass of developers abandoning JavaScript for this - especially considering Microsoft's past attitude to the web and Internet Explorer. The amount of bridges they must have burned is quite large at this stage. If I'd suffered for years working around IE6's issues, the last thing I'd do is switch to Microsoft's new web language.

Overall though, I think it was worthwhile for the company to use TypeScript. The pros outweigh the cons, especially once you identify the issues with TypeScript and develop coding standards to avoid them. As a developer I would have preferred CoffeeScript as a JavaScript replacement but I can see how it would be easier to shift Java developers over to TypeScript. I think its given them a lot of security that they wouldn't have had with JavaScript.
    
[TypeScript]:http://www.typescriptlang.com
[Backbone]:http://www.backbonejs.org
[WebStorm]:http://www.jetbrains.com/webstorm/
[Visual Studio]:http://msdn.microsoft.com/en-us/vstudio/aa718325.aspx
[Sublime Text]:http://www.sublimetext.com
[Definitely Typed]:https://github.com/borisyankov/DefinitelyTyped