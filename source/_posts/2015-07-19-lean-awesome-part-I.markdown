---
layout: post
title: Making awesome software with Lean principles - Part 1 
comments: true
categories: 
- Software Design
date: 2015-07-19 17:12:49
published: true
---

<a href="http://midsussextriclub.com"><img src="http://i49.photobucket.com/albums/f299/hookmike/MSTC%20_logo_yellow_280_zps4tnfm4qj.jpg" class="alignleft" alttext="MSTC Logo" /></a>

I've been a keen amateur triathlete and member the [Mid Sussex Tri Club](http://midsussextriclub.com) for a couple of years. Its a relatively small club but there is a surprising amount of administration involved in running it and after chatting to some of the clubs committee members it was clear that the admin overheads were becoming a burden. I could see the potential for software to help lessen the burden so I offered to take on the club's website and add some features to it.

Fast forward a year and we have a greatly enhanced website with new features that have helped grow the club without taking up more of people's time. Throughout making these changes I've applied Lean software principles which have really helped me. So in this post I'm taking a look at the first 3 principles of Lean and how these have assisted on this project.

##Eliminate waste ##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
Lean philosophy regards everything not adding value to the customer as waste.
{% endblockquote %}

I had great motivation to ensure that this principle was applied as I've been making all the changes during my spare time. Its amazing what you can achieve in a short amount of time with a bit of planning. For example just last week I was able to add 2 new payment options to the site in an hour. The key techniques I've used to help eliminate waste are:

- Make sure I understand what the key stakeholder wants to achieve from the feature before implementing it
- Design directly in HTML - I've designed all the new user interfaces directly in HTML so when it comes to implementing the feature I just need to enhance the existing HTML with the dynamic data elements
- Use third parties to do the heavy lifting (ie. don't reinvent the wheel). For this project the key third party systems used are [Umbraco](http://umbraco.com/) for content management, [Bootstrap](http://getbootstrap.com/) for styling and [GoCardless](https://gocardless.com/) for online direct debit payments 

##Amplify learning##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
Software development is a continuous learning process with the additional challenge of development teams and end product sizes.
{% endblockquote %}

Working in a development team of one on this particular project obviously made knowledge sharing a non issue, however there were opportunities to ensure learning was applied instead of doing things the old way. Integrating the GoCardless payment system presented a great opportunity for learning. The club were reluctant to extend the existing paypal based solution due to the fees involved, with paypal charging nearly 4% per transaction.  So I did some research and found that the GoCardless direct debit service charged just 1% per transaction. 

We decided to trial the system and I was pleasantly surprised by how intuitive it was it integrate with. They have [top notch documentation of their API](https://developer.gocardless.com/) and client libraries for a wide range of platforms including ours, .NET. I had a test payment process integrated with our website within a couple of hours, their support was also excellent and helped clarify the few points I wasn't clear on regarding setting up specific redirect URLs.

The system has been up and running for around 6 months now and the only issue we've had was with some intermittent error responses. Again their support team looked into the problem as soon as I raised it and implemented a fix. I'm glad that I took the time to learn how to integrate with GoCardless as it has saved our small club hundreds of pounds in fees already!

##Decide as late as possible##

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Lean_software_development Lean software development %}
As software development is always associated with some uncertainty, better results should be achieved with an options-based approach, delaying decisions as much as possible until they can be made based on facts and not on uncertain assumptions and predictions.
{% endblockquote %}

Taking this approach has helped me avoid the common 'analysis paralysis' problem whereby you try and solve too many problems at once, tie yourself in knots and deliver nothing! I deferred the implementation of several tricky  features and was pleasantly surprised that the solutions turned out to be more straight forward than I'd originally anticipated. 

For example we wanted to add an online entry system for several club events however the we needed to accept entries from club members, affiliated club members or guests. My initial thinking was to have an entry form for each type of entrant with some reporting so the event organisers could see who had entered. This sounded like a big feature that would take weeks to implement. So I started by just adding a simple form for existing members to enter our Duathlon event (fortunately no guests were allowed for our Duathlon!). Later on I returned to the problem of adding guest entries and suddenly it became obvious to me that guests could be members of the website as well just with a different role to limit their access. Then the same entry forms could be used for everyone and I wouldn't have to add any new reporting. Deferring the decision for how to implement this feature saved me loads of time and resulted in a simpler system, wins all round!

##Stay tuned for part II...##

The first three principles of Lean development have really helped me deliver on this project but there are still four more principles to go, so if you've found this interesting check back soon for part II in the series!