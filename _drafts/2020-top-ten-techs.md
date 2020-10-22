---
layout: post
title: Top Ten Tools, Libraries, and Languages -- 2020
categories: thoughts
image: '/assets/images/space-1.jpg'
imageAttributeUrl: 'https://unsplash.com/@tavi004'
imageAttributeName: 'Octavian Rosca'
---

A rundown of software development tools, libraries, and languages that I've loved working with in 2020.

## 10. Angular

_My go-to client-side framework._

[Home](https://angular.io/) / [GitHub](https://github.com/angular/angular) / [Getting Started](https://angular.io/guide/setup-local)

The options for front-end frameworks are plentiful, but the three most popular are React, Vue, and Angular (also see Svelte and Elm).

For several reasons around scalability, Angular has been a common choice for large companies. However, popularity has been slowly declining over the last couple of years as React has gained more [mindshare](https://trends.google.com/trends/explore?cat=31&date=today%205-y&q=Vue.js,React,Angular).

That being said, I think Angular is an excellent option for small-scale projects too. There is lots of documenation online and the Angular has an excellent [tutorial](https://angular.io/tutorial). The biggest hurdle with Angular is its steep learning curve.

My favorite parts of Angular are the number of out-of-the-box components, support for observables with RxJs, and its phenomenal command-line utility `ng`.

## 9. Pop_OS!

_A beginner friendly Linux distribution._

[Home](https://pop.system76.com/) / [GitHub](https://github.com/pop-os) / [Getting Started]()

I've been working with Pop_OS! for 3 years and I can't imagine using any other distro anytime soon. The OS itself is a downstream branch of Ubuntu, so it will be familiar to users coming from a Debian-like ecosystem. The user interface uses Gnome instead of Unity. I highly recommend installing Gnome Tweaks and configuring things as you'd like.

Version 20.04 shipped out of the box with a workspace management system, which auto aligns windows for you as you open them. I'm impressed with the number of features the team at System76 is able to pack into each release.

Overall, Pop is an excellent beginner-friendly Linux OS that ships with a lot of great tools right out of the box. Fans of Unix, Windows, and Mac-OS systems will all find a home here.

## 8. Jekyll

_A simple framework for building web apps with markdown content._

[Home](https://jekyllrb.com/) / [GitHub](https://github.com/jekyll/jekyll) / [Getting Started](https://jekyllrb.com/docs/)

Due to issues with discoverability and SEO performance with my Angular-built blog, I needed to switch to a non-SPA system.

A co-worker recommended Jekyll and I was quickly impressed after trying out a tutorial. It comes with an elegant out-of-the-box theme that requires no tweaks to get running. The documentation is pretty solid and its also very configurable. I was even able to create my own theme from the ground up using [Materialize CSS](https://materializecss.com/).

It's important to note that some Ruby knowledge would be helpful going in, but it is not required by any means.

Time will tell how well it handles SEO-optimizations, but since the Markdown+HTML+SASS+JS all gets compiled to plain HTML+CSS+JS, I have no fears about discoverability.

## 7. Rocket

_An elegant and well-documented Rust web framework._

[Home](https://rocket.rs/) / [GitHub](https://github.com/SergioBenitez/Rocket) / [Getting Started](https://rocket.rs/v0.4/guide/)

Quick Disclaimer: Rocket currently requires the nightly version of Rust to build and has no async/futures support as of now. Support for async and stable Rust are planned for sometime soon, but for now, it should not be used in production. See [AWWY](http://www.arewewebyet.org/topics/frameworks/) for alternative web server frameworks for Rust.

Rocket is the most fun web server I've ever used. It has built-in support for lots of different things you might want to use a web framework for. And the best part is that it doesn't require any hacky workarounds to be useful. Everything feels natural to the Rust ecosystem and Rust's compiler safety is preserved.

## 6. C#

[Home]() / [GitHub]() / [Getting Started]()

_A powerful, easy to use, and fun object-oriented programming language with excellent tooling and library support._

I view C# as an improved form of Java. The runtime (CLR) is much easier to install than the JDK and the tooling around building, running, testing, and deploying C# code is much simpler than the Java equivalents.

Everything required to build C# programs can be installed from the [dotnet download page](https://dotnet.microsoft.com/download).

Some excellent language enhancements included in the upcoming C# 9 release make it my number 1 recommendation for an object-oriented language.

## 5. ASP .NET

[Home]() / [GitHub]() / [Getting Started]()

_An extensive web framwework for C#._

## 4. NuShell

[Home]() / [GitHub]() / [Getting Started]()

_An improved terminal experience._

## 3. VS Code

[Home]() / [GitHub]() / [Getting Started]()

_The last editor I will ever need._

## 2. F#

_A functional programming language with excellent tooling, libraries, documentation, and features._

I use F# for solving [Project Euler]() math problems. Check out my repository of simple solutions [here]().

For an example project I built using F#, take a glance at [Formula Team Manager](https://github.com/b-vennes/formula-team-manager) on GitHub.

## 1. Rust

_The best programming language I've ever used._

Rust is in a class of its when it comes to programming languages. It has everything you could possibly need from a programming language (except a low learning curve).

Rust's execution performance rivals C and C++, while also having a modern and user friendly syntax.

Its solution to memory management and garbage collection actually requires the developer to put some thought into the strcture and architecture of their code. Although sometimes feeling like a blocker, the compiler forces the developer to write code that is thread safe and thoughtful about variable state and ownership.

No other langauge improves my ability to write better code in all aspects of programming like Rust does. If you have not tried it, I highly recommend taking a look for yourself.

[The Rust Book](https://doc.rust-lang.org/book) is a great place to start!