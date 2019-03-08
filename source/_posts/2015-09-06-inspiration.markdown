---
layout: post
title: Inspiring software design
comments: true
categories: 
- Software Design
date: 2015-09-06 18:07:49 +01:00
published: true
---

[<img src="/images/posts/idea.jpg" class="alignleft" title="Ideas lightbulb" />](http://www.603copywriting.co.uk/blog-post-ideas/)

The software design process can sometimes be difficult to quantify. From an external perspective of a development team, requirements go in and working software comes out. But what exactly happens in the middle to turn the stakeholders hopes and dreams into reliable, executable code? 

At its core software design is a methodical, incremental engineering process and there are no short cuts to this process if we want to produce good results. However making really great software that will stand the test of time often requires a little something extra, a bit of inspiration. So where exactly does that spark come from and how can we help create the conditions to nurture it?      

##A sticky design problem

I've recently been working on a feature to enable users to update their profiles through a web application. This is a pretty standard problem however some extra complexities were introduced by the target platform. Chief among those was the user interface which needed to be asynchronous and fit in with the applications existing front end [SPA framework](https://en.wikipedia.org/wiki/Single-page_application). 

I like to implement using the '[outside in](http://programmers.stackexchange.com/questions/166409/tdd-outside-in-vs-inside-out)' approach so began with the user interface, adding one section at a time then abstracting any common patterns to make them reusable. However, after a day at the coal face writing code I took a step back and wasn't pleased with my results. The code was reaching a point where I was struggling to keep track of the workflow in my own head which doesn't exactly bode well for the poor guy that would inevitably need to modify it 6 months down the line. The trouble was no matter how hard I stared at those lines of code a more elegant and simple solution would not present itself!

The very next day I woke up feeling sick and not be able to get into the office to finish implementing the UI. However, it may well have been the most productive 'time off' work I've had in some time. As I recuperated watching Hobbits and Orcs do battle on a grand scale my mind drifted back to the problems I'd been having yesterday. It suddenly became clear to me that I had been abstracting at too low a level and trying to fit all of the behaviour into a single javascript module. However if I just created a single module per user profile element I could create a common 'interface' for each module and easily iterate over them in the main module. 

##Emergent architecture 

This new approach had all the hallmarks of a good design as my own reaction was along the lines of 'that is so obviously better, why didn't I just do it like that in the first place?!' That is a pertinent question, and one that has probably troubled many a software developer. However my feeling is that often it is not possible to skip the 'bad design' and go straight to the better solution because you don't know what all the problems are going to be until you start implementing. Thomas Edison summed this up nicely in one of his most famous quotes:

{% blockquote Wikiquote https://en.wikiquote.org/wiki/Thomas_Edison Thomas Edison %}
What it boils down to is one per cent inspiration and ninety-nine per cent perspiration.
{% endblockquote %}

While its certainly true that experience can help you see patterns and spot the problems earlier I think it is a mistake to try and design everything meticulously up front. There is bound to be some devil in the detail which makes you pivot on the design halfway through so a lot of that up front time will have been wasted. 

I find it is best to have a balance, with an initial high level draft plan for the implementation usually implemented through UML modelling. This helps guide lower level design decisions and keep one eye on the bigger picture. To make the lower level design decisions I try and stay agile, adapt the code as I'm going along and keep complexity at a minimum. 

##Encouraging those aha! moments

Often my best ideas are formed away from the computer, hours or even days after the initial problem arose. The sub conscious mind is an amazing tool which can help you solve many intractable issues if you can give it the space and time to join up those dots. Despite what our employment contracts may say, software development really isn't a 9 to 5 job. In fact working 'harder' during those office hours can sometimes be counter productive when it comes to making great software. Good decisions and great solutions are formed in relaxed and fresh minds, for that reason I'd have to list my bike as one of my most essential software design tools!

As developers we can encourage good software design by focussing on a couple of key elements:

- Collaborate on design, decisions should be guided by [the ten commandments of egoless programming](http://blog.codinghorror.com/the-ten-commandments-of-egoless-programming/) 
- Ensure your estimates include enough contingency to allow for rapid prototyping and iterations during implementation.
- Stay pragmatic; aim high but know that perfection is subjective. Establishing coding standards can help guide when a design is good enough 

Do you have any particular techniques to help inspire your software designs? If so I'd love to hear about them and try them out myself!
