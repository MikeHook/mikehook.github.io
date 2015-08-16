---
layout: post
title: Making awesome software with Lean principles - Part II
comments: true
categories: 
- Software Design
date: 2015-07-26 18:07:49 +01:00
published: true
---

<img src="http://i49.photobucket.com/albums/f299/hookmike/74078639-2d6b-4179-883d-4be7d28c19c7_zpsbpo9illj.jpg" class="alignleft" title="Awesome image by Roberlan https://www.flickr.com/photos/roberlan" />

In the [first part of Making awesome software with Lean principles](http://bakingwebsites.co.uk/2015/07/19/lean-awesome-part-I/) I started looking at how [Lean principles](https://en.wikipedia.org/wiki/Lean_software_development) have helped me while working on the [Mid Sussex Tri Club website](http://midsussextriclub.com/). 

The first three principles **Eliminating waste**, **Amplifying learning** and **Deciding as late as possible** are all fundamental to help guide software development. However they only form part of the picture, there are still four more principles remaining that help guide our decisions so let's take a look at them now.

##Deliver as fast as possible##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
In the era of rapid technology evolution, it is not the biggest that survives, but the fastest. The sooner the end product is delivered without major defects, the sooner feedback can be received, and incorporated into the next iteration.
{% endblockquote %}


I've seen the benefits of an iterative approach over and again throughout my professional life so I regard this as an essential principle to apply to all software projects. I put in place a pretty slick release process as one of the first changes I made and have been reaping the benefits ever since. Basically any check-ins on the master branch of the source code repository are immediately deployed to the website. I've already blogged about how I set-up this ['zero-click' deploy process](http://bakingwebsites.co.uk/2014/07/02/automated-azure-deployments/) and it has worked largely without problems since set-up.

However, this technique must be used with caution and certainly in tandem with the '**Build quality in**' principle. It is essential to ensure that a suitable branching process is in place too and changes can only be deployed once they have been tested. If you are working in a larger team I'd recommend limiting write access to the master branch to a single person who can coordinate the deployments. While automation is great for productivity people still need to understand how it all works and retain control of the systems. 

##Empower the team##
      
{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
The lean approach favors the aphorism "find good people and let them do their own job," encouraging progress, catching errors, and removing impediments, but not micro-managing.
{% endblockquote %}

I have been fortunate to be involved with an organisation that trusted me to make the technical decisions. I presented my ideas on the features to implement and got feedback on any changes they thought may be needed. So I was empowered to make the changes as and when they were needed. However I also think it is important to 'bring the stakeholders with you' when making the changes. This has several benefits, not only does it help ensure that the features are relevant, it also gives me some good candidates for people to test the system before it goes live, which brings me neatly onto the next principle!

##Build quality in##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
Conceptual integrity means that the system’s separate components work well together as a whole with balance between flexibility, maintainability, efficiency, and responsiveness.
{% endblockquote %}

As I mentioned earlier, this principle acts as a counterweight to some of the other Lean principles, such as **Deliver as fast as possible**, to help ensure that changes are not rushed and the software doesn't accumulate bugs and brittle implementations. For this project I've not had the luxury of any other developers who could review the code, however I've still ensured that changes are functionally reviewed before deploying them to the live site. To facilitate this I've setup a 'staging' environment using the same deployment technique running off a separate 'staging' repository branch. I first implement the changes on the staging branch and only merge them into the live 'master' branch once someone has tested them out on the staging website.

On one occasion I skipped the staging process and fate taught me a lesson! The update was to use an Umbraco plugin to automatically resize images but little did I know that there was a memory leak in the Umbraco plugin which meant the site quickly went over its memory allocation and Azure automatically took it offline. That was a nasty surprise to find on the live site and some load testing in the staging environment would certainly have helped! 

##See the whole##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
By decomposing the big tasks into smaller tasks, and by standardizing different stages of development, the root causes of defects should be found and eliminated.
{% endblockquote %}

There are two key elements to the last principle, **See the whole**. I've already talked about how the staging and live environments are standardized, in my case this was a fairly trivial exercise made easy by modern hosting tools such as Azure and github that enable multiple environments to be set up at very low cost. The small fee to run an extra Azure database is well worth the value it adds for  the club.

The second element recommends splitting tasks into smaller pieces and is certainly one that I'd advocate. Limiting the number of changes made at any one time really helps with tracking down issues. Its a practice which I've learnt the hard way over the years, it can be tempting to try and 'fix the world' when you get your hands on a code-base and hammer out a high number of changes in a short time. However, not only does this massively increase the probability for bugs, it also makes finding them much more difficult. Looking for a bug in a commit with 50+ changed files isn't much fun! 

I've used the [github issue tracker](https://github.com/MikeHook/MSTC/issues) on this project, which may seem a bit odd when there is no one else to share the work with, however it has helped keep me focussed on what I've been trying to achieve, also its nice to get the satisfaction of pressing the 'Closed' button after fixing each issue! 

##Lean == Awesome ##

I hope you've enjoyed reading about my experiences with Lean principles. I'd love to hear about any opinions you've got regarding the way I've interpreted the principles or how you've used the principles to make your own awesome software, so feel free to comment on the posts below! 




