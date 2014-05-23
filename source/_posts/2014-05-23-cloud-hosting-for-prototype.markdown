---
layout: post
title: "Cloud hosting for a prototype project"
date: 2014-05-23 16:04:20 +0100
comments: true
categories: 
- Hacking
published: true
---

<img src="http://imagizer.imageshack.us/v2/320x240q90/838/0fsb.jpg" class="alignleft" alttext="Square peg - Round hole"  />

<br/><br/>
Today I am investigating cloud hosting platforms for a green field web application, there are quite a few platforms out there now, however I'll be looking at some of the big hitters as they are tried and tested in the marketplace. 

The full technology stack is still under review so the hosting capabilities need to be fairly flexible however the requirements we know for sure are:

- Able to spin up multiple instances to run testing, staging and production environments
- Low cost, ideally zero for testing / staging as this is grass roots project!
- Flexibility to host a range of technologies
- High reliability / speed (this should be a given for any hosting environment)

##Contenders ready!

I'm going to investigate 3 platforms against the main requirements above, Windows Azure, Amazon Web Services and Heroku. Each have their advantages, so lets find out which will be the best fit for our project.

###Azure

Azure is the Microsoft Cloud offering. As such it has a Microsoft technologies leaning but is by no means limited to their stack. 

####OS / Languages / DBs
- Windows or Linux
- .NET, Node.js, Java, PHP, Python, Ruby
- Native DBs: Azure SQL Server
- Third party DBs: Neo4j, MySQL, MongoDB (fiddly)

####Pricing
- 30 day free trial then,
- $10/month per site
- $2.50/month per DB

###AWS

Amazon Web Services were one of the first cloud based hosting solutions out there. It is a mature platform with many options but lets see whether the acronyms compare favourably.

####OS / Languages 
- Windows or Linux
- .NET, Java, PHP, Python, Ruby
- Native DBs: SQL Server, MySQL, Oracle, PostgreSQL
- Third party DBs: Neo4j, MongoDB, RavenDB (basically anything you can run on VM)

####Pricing
- 12 months free for 1 instance / DB
- $15/month per site
- $40/month per DB

###Heroku

Heroku is more of a grass roots developer led cloud platform. It is well suited to an open source license free stack but will this be suitable for our application? 

####OS / Languages 
- Windows or Linux
- Node.js, Java, Python, Ruby
- Native DBs: Postgres
- Third party FBs: neo4j, MySQL 

####Pricing
- 1 dyno free, $35/month per extra instance
- Dev DBs free (Up to 10K rows), Basic $9/month

##And the winner is...

The technology stack hasn't been chosen yet so we will just need to wait to see you the winner is! 








