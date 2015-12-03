+++
date = "2015-04-01T20:46:41+01:00"
tags = ["Javascript", "Angular"]
title = "My tiny skeleton for an angular app"

+++

It happens many times that I have an urgent need to quickly check some code that just came to my mind – angular feature, new plugin or library.

Sometimes it is about prototyping part of UI. Something really tiny, but still – with such requirements as libraries, tools and all that stuff. So what do I do? I quickly download HTML5 boilerplate, Foundation for styles and grid (if i need to display something) and then all angular stuff, libs… It takes time and is totally worthless. I have looked for skeletons over the web, but none of them fulfill my requirements of lightweight, simplicity and set of tools. I discovered yeoman – cool idea but I can’t find there what I need.

So I made my own template. It’s based on HTML5 boilerplate v.4.something and contains the following preconfigured tools:

- Grunt – as an automatic build system
- Karma – as a tests runner
- Jasmine 2 – as a testing framework

The above are handled by npm (Node Package Manager). So you need to type npm install.

And then the core set:
Angular 1.3 as an angular :p mvc framework
Base – as an awesome lightweight css base (thanks for [Matthew Hartman](http://matthewhartman.com.au/)).

For the source code check out [github](https://github.com/czytelny/ng-kickstarter).

