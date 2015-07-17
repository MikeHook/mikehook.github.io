---
layout: post
title: xxx
comments: true
categories: 
- Progress
date: 2015-04-23 18:07:49 +01:00
published: false
---

#Get Lean and make awesome software #



##Eliminate waste ##

*Lean philosophy regards everything not adding value to the customer as waste*

I had great motivation to ensure that this Lean principle was applied as the project has been carried out entirely during my spare time. Its amazing what you can achieve in a short amount of time with a bit of planning. For example just last week I was able to add 2 new payment options to the site in an hour. The key techniques I've used to help ensure waste is eliminated are:

- Make sure I understand what the key stakeholder wants to achieve from the feature before implementing it
- Use third parties to do the heavy lifting or put in another way don't reinvent the wheel. For this project the key third party systems used are [Umbraco](http://umbraco.com/) for content management and [GoCardless](https://gocardless.com/) for online direct debit payments 

##Amplify learning##

*Software development is a continuous learning process with the additional challenge of development teams and end product sizes*

Working in a development team of one on this particular project obviously made knowledge sharing a non issue, however there were opportunities to ensure learning was applied instead of doing things the old way. Integrating the GoCardless payment system presented a great opportunity for learning. The club were reluctant to extend the existing paypal based solution due to the fees involved, with paypal charging nearly 4% per transaction, so I did some research and found out that GoCardless service charged just 1% per transaction. 

We decided to trial the system and I was pleasantly surprised by how intuitive it was it integrate with GoCardless. They have [top notch documentation of their API](https://developer.gocardless.com/) and client libraries for a wide range of platforms including ours, .NET. I had a test payment process integrated with our website within a couple of hours, their support was also excellent and helped clarify the few points I wasn't clear on regarding setting up specific redirect URLs.

The system has been up and running for around 6 months now and the only issue we've had was with some intermittent error responses. Again their support team looked into the problem as soon as I raised it and implemented a fix. I'm glad that I took the time to learn how to integrate with GoCardless as it has saved our small club hundreds of pounds in fees already!

##Decide as late as possible##

*As software development is always associated with some uncertainty, better results should be achieved with an options-based approach*

???

##Deliver as fast as possible##

*In the era of rapid technology evolution, it is not the biggest that survives, but the fastest. The sooner the end product is delivered without major defects, the sooner feedback can be received, and incorporated into the next iteration.*

I've seen the benefits of an iterative approach over and again throughout my professional life so I regard this as an essential principle to apply to all software projects.

Slick release process for the WIN! (link to previous blog post)

##Empower the team##
      
*The lean approach favors the aphorism "find good people and let them do their own job," encouraging progress, catching errors, and removing impediments, but not micro-managing.*

I have been fortunate to be involved with an organisation that trusted me to make the technical decisions. I presented my ideas on the features to implement as suggestions and got feedback on any changes they thought may be needed. 

IMPORTANT TO 'Bring the stakeholders with you' when making the changes

##Build quality in##

All new features are first tested on a staging environment. Would like some automated testing in the project in future

##See the whole##

*By decomposing the big tasks into smaller tasks, and by standardizing different stages of development, the root causes of defects should be found and eliminated.*

Features fully implemented one at a time, used an issue tracker to keep on top of requests.



