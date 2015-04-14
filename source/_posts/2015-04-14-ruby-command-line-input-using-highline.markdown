---
layout: post
title: "Ruby Command Line Input Using Highline"
date: 2015-04-14 23:24
comments: true
categories: Development
keywords: ruby, highline, command line
---

I've been using Rakefiles a lot recently to automate tasks. I find it really useful in comparison to shell scripting as I can run the rakefiles under different OSes (OS X and Windows) and have it behave the same in all cases.

I was building a simple [script][] to automate the generation of pgn files for my chess games. I wanted to be able to enter the details of the games on the command line and then have my script output a pgn file. 

I didn't want to have to go messing about with low level operations such as `puts` and `gets` so I searched for something better. I found the [Highline][] gem for this and was really happy with it.

It allows for a variety of input formats

* To display a prompt to the user and then store the input in a variable use
    `event = ask("Event Title: ")`

* You can specify default values and the default will be listed as part of the prompt. On the command line simply hit enter to get the default value.
    `timeControl = ask("TimeControl: ") { |q| q.default = "5400+15" }`

* You can create a menu from an array of values using the choose function.
    `result = choose("1-0", "1/2-1/2", "0-1", "*")`

These can be customised and there are a lot more options available such as entering passwords. If you are doing text input in Ruby, I would advise checking it out.

[script]: https://github.com/gerardcondon/pgn/blob/master/Rakefile
[Highline]: https://github.com/JEG2/highline