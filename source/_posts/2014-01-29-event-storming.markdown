---
layout: post
title: "Event Storming a distributed solution"
date: 2014-01-29 17:38:27 +0000
comments: true
categories:
- Software Design
published: true
---

Software design and development techniques are constantly evolving, making the discipline a fascinating area to work in. Over the time I've been working as a developer I've seen various approaches to design from Code First, Data Driven to Domain Driven (DDD). My preference is to consider the scope of the problem first and design a solution to match and DDD is a good fit for this. However when looking at enterprise level problems we need to give more consideration to how a problem will scale... enter Event Storming. 

## Distributed design

I work in a development team which is responsible for several key business capabilities, there are already some software solutions in place for these capabilities however they were designed several years ago to solve problems at a scale at least an order of magnitude below where we currently find ourselves. Our stakeholders have expressed a desire that we could redesign a solution which would meet our current needs better, basically it needs to work **faster**, do *just* what is needed and ideally **scale** so that it will still perform well at the next order of magnitude up.

We decided to have an Event Storming session to help us analyse the problem and come up with a high level distributed solution. We carried out the session in a couple of phases, first we reviewed the current solution, then we looked at the primary goals for the redesign then finally we started event storming a solution. Here are the team hard at work mid-storm:

<img src="http://imageshack.com/a/img542/5968/1l37.jpg" class="alignleft" alttext="Octopress Logo"  />

## Collaborative analysis

The session first involved us all writing down on post-it notes business events that had some relevance to the feature we were analysing. We then stuck all these onto a big board and de-duped them. It was a bit tricky to order the post-its at this stage so we went for a loose left to right time sequence.

Following this we got the post-it stack back in action and wrote down each of the commands that could have some influence on the events on the board. Then we linked each of the commands to events on the board, this wasn't always a one-to-one relationship which presented some practical problems, however on the whole I think working with physical post-its was far better than using something like Visio as its more democratic. Finally we arranged the post-its into discrete pieces of behaviour and mapped out the interactions between them.  

It was important that we recognised the difference between an event and a command during the exercise. As a rule of thumb commands are carried out by users and events are raised by the system to indicate something has happened. For example the *Create Product* command is raised when the user saves a product entry, whereas *'Product Created'* event is raised after the transaction has finished and a product has been persisted in the system. As I alluded to earlier there is no hard one-to-one relationship here, a single command can often result in multiple events being raised and vice versa. Its important to make sure the distinction is clear to everyone before the session begins, it saves a lot of time in the long run!  

The results of our session can be seen in the picture below:
<img src="http://imageshack.com/a/img34/1388/bbid.jpg" class="alignleft" alttext="Octopress Logo"  />

## A virtual solution

By the end of the session we had a virtual solution figured out that could be implemented with discrete modules communicating exclusively via messaging. I was pleasantly surprised by the elegance of the solution, by focussing on the interactions between the systems and isolating responsibilities it was clear that we only needed to pass a small amount of data between each module and could effectively encapsulate the behaviour. However Event Storming is just one technique and I wouldn't suggest it is a magic bullet design technique, these are the main pros and cons as I see it.

###Advantages

- Helps focus on the interactions between elements of a distributed system
- The whole team is involved at the analysis stage and visualize the same proposed solution
- Encourages scalable solutions

###Drawbacks

- Reliant on domain knowledge of team members to surface events / commands 
- Design can get lost in a sea of post-it notes!
- Need to transfer quickly to implementation tasks to avoid losing insights 

I'd be interested to hear of your experiences of event storming and how effective you have found it to be?





    
  

          




