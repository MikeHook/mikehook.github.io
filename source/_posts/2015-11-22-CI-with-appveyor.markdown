---
layout: post
title: Continuous Delivery to Azure with AppVeyor
comments: true
categories: 
- Progress
date: 2015-11-22 18:07:49 +01:00
published: false
---

[<img src="https://googledrive.com/host/0Bx-8nw9dhAQcN1lWbU1SLW91bEk/AppVeyorLogo.png" class="alignleft" title="AppVeyor" />](http://www.appveyor.com/)

I've been using [Kudu to automate my website deployments](http://bakingwebsites.co.uk/2014/07/02/automated-azure-deployments/) from Github to Azure hosting for quite a while. Its worked out great but there are some limitations with it, primarily lack of control over whether changes are deployed, its very much an all or nothing tool.

The complexity of the web application reached a level where I wanted to ensure that the code builds and passes some automated tests before it is deployed. I couldn't see any way to incorporate those steps into a deployment pipeline with Kudu so decided to give AppVeyor a go. I'd heard about it on [Scott Hanselmans blog](http://www.hanselman.com/blog/AppVeyorAGoodContinuousIntegrationSystemIsAJoyToBehold.aspx) a while ago (my font of all .NET knowledge) and as it is free for open source projects I'd been itching to give it a go! 

##Build it, build it

blah

##Tasty Tests  

blah 2

##AppVeyor, meet Azure

blah3
