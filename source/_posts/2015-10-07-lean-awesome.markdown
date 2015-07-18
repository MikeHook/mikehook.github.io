---
layout: post
title: xxx
comments: true
categories: 
- Progress
date: 2015-07-10 18:07:49 +01:00
published: false
---

#Making awesome software with Lean principles - Part 1 #

I've been a keen amateur triathlete and member the [Mid Sussex Tri Club](http://midsussextriclub.com/) for a couple of years. It is a relatively small club with around 100 members but there is a surprising amount of administration involved in running the club. After chatting to some of the clubs committee members it was clear that they were struggling to find time to run the club with work, family and training commitments swallowing up a great deal of time. I could see the potential for software to help make a lot of their problems go away so I offered to take on the clubs website and add some features to it.

Fast forward a year and we have a greatly enhanced website with some great features that have helped grow the club whilst reducing the admin overheads. Throughout making these changes I've applied Lean software principles and I think these have really helped make these changes effectively. In this post I'm taking a look at the first 3 principles of Lean and how these have helped on this project.

##Eliminate waste ##

*Lean philosophy regards everything not adding value to the customer as waste*

I had great motivation to ensure that this Lean principle was applied as the project has been carried out entirely during my spare time. Its amazing what you can achieve in a short amount of time with a bit of planning. For example just last week I was able to add 2 new payment options to the site in an hour. The key techniques I've used to help ensure waste is eliminated are:

- Make sure I understand what the key stakeholder wants to achieve from the feature before implementing it
- Design directly in HTML - I've designed all the new user interfaces directly in HTML so when it comes to implementing the feature I just need to enhance the existing HTML with the dynamic data elements
- Use third parties to do the heavy lifting or put in another way don't reinvent the wheel. For this project the key third party systems used are [Umbraco](http://umbraco.com/) for content management, [Bootstrap](http://getbootstrap.com/) for styling and [GoCardless](https://gocardless.com/) for online direct debit payments 

##Amplify learning##

*Software development is a continuous learning process with the additional challenge of development teams and end product sizes*

Working in a development team of one on this particular project obviously made knowledge sharing a non issue, however there were opportunities to ensure learning was applied instead of doing things the old way. Integrating the GoCardless payment system presented a great opportunity for learning. The club were reluctant to extend the existing paypal based solution due to the fees involved, with paypal charging nearly 4% per transaction, so I did some research and found out that GoCardless service charged just 1% per transaction. 

We decided to trial the system and I was pleasantly surprised by how intuitive it was it integrate with GoCardless. They have [top notch documentation of their API](https://developer.gocardless.com/) and client libraries for a wide range of platforms including ours, .NET. I had a test payment process integrated with our website within a couple of hours, their support was also excellent and helped clarify the few points I wasn't clear on regarding setting up specific redirect URLs.

The system has been up and running for around 6 months now and the only issue we've had was with some intermittent error responses. Again their support team looked into the problem as soon as I raised it and implemented a fix. I'm glad that I took the time to learn how to integrate with GoCardless as it has saved our small club hundreds of pounds in fees already!

##Decide as late as possible##

*As software development is always associated with some uncertainty, better results should be achieved with an options-based approach*

Taking this approach has helped me avoid the common 'analysis paralysis' problem whereby you try and solve too many problems at once, tie yourself in knots and deliver nothing! I deferred the implementation of several tricky  features and was pleasantly surprised that the solutions turned out to be more staight forward than I'd originally anticipated. 

For example we wanted to add an online entry system for several club events however the we needed to accept entries from club members, affiliated club members or guests. My initial thinking was to have an entry form for each type of entrant with some reporting so the event organisers could see who had entered. This sounded like a big feature that would take weeks to implement. So I started by just adding a simple form for existing members to enter our Duathlon event (fortunately no guests were allowed for our Duathlon!). Later on I returned to the problem of adding guest entries and suddenly it became obvious to me that guests could be members of the website as well just with a different role to limit their access. Then the same entry forms could be used for everyone and I wouldn't have to add any new reporting. Deferring the decision for how to implement this feature saved me loads of time and resulted in a simpler system, wins all round!

##Deliver as fast as possible##

*In the era of rapid technology evolution, it is not the biggest that survives, but the fastest. The sooner the end product is delivered without major defects, the sooner feedback can be received, and incorporated into the next iteration.*

I've seen the benefits of an iterative approach over and again throughout my professional life so I regard this as an essential principle to apply to all software projects. I put in place a pretty slick release process as one of the first changes I made and have been reaping the benefits ever since. Basically any check-ins on the master branch of the source code repository are immediately deployed to the website. I've already blogged about how I set-up this ['zero-click' deploy process](http://bakingwebsites.co.uk/2014/07/02/automated-azure-deployments/) and it has worked largely without problems since set-up. 

The only problem I encountered was after deploying an update to automatically resize images, little did I know that there was some kind of memory leak in the Umbraco plugin which meant the site quickly went over its memory allocation and Azure automatically took it offline. That was a nasty surprise and in hindsight testing the feature in a separate environment first would have helped! I wouldn't advocate this release process for bigger teams as there would be an increased risk of deploying code by mistake however it is a good fit for my needs and has helped me deliver changes quickly and reliably.

##Empower the team##
      
*The lean approach favors the aphorism "find good people and let them do their own job," encouraging progress, catching errors, and removing impediments, but not micro-managing.*

I have been fortunate to be involved with an organisation that trusted me to make the technical decisions. I presented my ideas on the features to implement and got feedback on any changes they thought may be needed. So I was empowered to make the changes as and when they were needed. However I also think it is important 'bring the stakeholders with you' when making the changes. This has several benefits, not only does it help ensure that the features are relevant, it also gives me some good candidates for people to test the system before it goes live, which brings me neatly onto the next principle!

##Build quality in##

*Conceptual integrity means that the system’s separate components work well together as a whole with balance between flexibility, maintainability, efficiency, and responsiveness.*

All new features are first tested on a staging environment. Would like some automated testing in the project in future

##See the whole##

*By decomposing the big tasks into smaller tasks, and by standardizing different stages of development, the root causes of defects should be found and eliminated.*

Features fully implemented one at a time, used an issue tracker to keep on top of requests.



