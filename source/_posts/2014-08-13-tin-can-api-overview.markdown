---
layout: post
title: "Tin Can API overview"
date: 2014-08-13 10:10:43 +0100
comments: true
categories: 
- Progress
published: true
---

<img src="http://imageshack.com/a/img539/1961/OgE8tH.jpg" class="alignleft" alttext="Tin Can API Logo" />

These are some notes from my reading on the Tin Can API, a specification for the transmission and storage of messages related to learning activities. The specification has been developed by parties in the e-learning industry and aims to be an elegant solution to enable learning systems to communicate with one another. 

All this information is available on the [Tin Can API website](http://tincanapi.com/), this is just my own [tl;dr](http://en.wikipedia.org/wiki/Wikipedia:Too_long;_didn't_read) type summary. 

###What is it?

- A RESTful web service specification
- Defines the structure for JSON messages which represent learning activities (Or other types of activity)
- Each message is called a **Statement**
- A statement consists of 3 parts: **Actor**, **Verb** and **Object** like "Mike read the Tin Can Explained article"
- Based on the [activity streams](http://activitystrea.ms/) specification developed by/for social networks

###Why is it good?

- More flexible version of the old SCORM standard
- Device agnostic - anything that can send HTTP requests can use the API
- Almost any type of content can be stored
- Decouples content from the LMS (Learning management system) by storing in a separate LRS (Learning Record Store)
- A single *statement* can be stored in multiple learning record stores 
- Allows potential for a learner owning their own content instead of their employers
- Data is accessible and easy to report on

###A statement example

	{
	    "actor": {
			"name": "Sally Glider",
        		"mbox": "mailto:sally@example.com"
     		},
		"verb": {
         		"id": "http://adlnet.gov/expapi/verbs/experienced",
         		"display": {"en-US": "experienced"}
         },
		"object": {
         		"id": "http://example.com/activities/solo-hang-gliding",
         		"definition": {
             		"name": { "en-US": "Solo Hang Gliding" }
         		}
     	}
	}

You can generate test statements with valid syntax using the [Statement Generator](http://tincanapi.com/statement-generator/)

Details of the valid message formats are given on the [full xAPI specification](https://github.com/adlnet/xAPI-Spec/blob/master/xAPI.md).

##The registry

- Contains definitions for verbs, activities, activity types, attachment types and extensions
- Should be referenced to in messages by URIs 
- A shared domain language for agreed definitions of terms
- Anyone can add their own definitions to [the registry](https://registry.tincanapi.com/)

##Recipes

- [Recipes](http://tincanapi.com/recipeshow-it-works/) provide a standardised way for people to describe activities
- Simplifies reporting as the same terms should be used to describe things
- A few recipes exist now, for example the [video recipe](https://registry.tincanapi.com/#profile/19)
- More recipes can be added by the community
- Helps keep the API flexible and useful for future applications that may not even exist yet!

Reading about the Tin Can API over the last few days and seeing it in action in the [Tessello application](https://www.tessello.co.uk) has really whet my appetite to work more with the specification. I can see great potential for systems that leverage this specification as it provides a flexible framework for messaging without being restrictive and gives us the basis for a common technical language to use to enable our systems to talk to one another. 