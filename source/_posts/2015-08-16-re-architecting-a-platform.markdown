---
layout: post
title: Re architecting a multi tenant platform
comments: true
categories: 
- Software Design
date: 2015-08-16 18:07:49 +01:00
published: false
---

Intro - explain the problem and goals for the re architecture

##High level plan##

Diagrams for before and after, explain major changes

<img src="http://i49.photobucket.com/albums/f299/hookmike/74078639-2d6b-4179-883d-4be7d28c19c7_zpsbpo9illj.jpg" class="alignleft" title="Awesome image by Roberlan https://www.flickr.com/photos/roberlan" />

##Implementing the changes##
      
- Analyse & document current behaviour
- Spec new behaviour
- Implement with BDD
- Incremental releases

##Pitfalls##

- 'Fix the world' syndrome
- Guard against new technical debt
- Regression - may break existing working features!
- Majority of value is long term with low visibility

##Conclusion ##

- Work in progress
- Real world priorities may change
- Delivered 3 major new features that are DONE DONE DONE! 


##A Developer always pays his debts##

Tell a story about a quest to the hallowed lands of technical excellence with zero bugs and no obsolete code.

Hit points for every dead line of code removed, test added and stored procedure deleted 
Level up by simplifying system behaviour, other things?

1st month
Transitioning from old style N-tier architecture all in web app: JS -> WebMethod -> Business Logic -> SProc DAL to 
modular service oriented architecture: JS -> WebApi -> Business Logic -> EF DAL
Shifted login, authentication and session management to API 

2nd month
Tooled up with nuget libraries for WebApi comms 
Shifted registration to API

3rd month
Tooled up with TeamCity
Shifted edit profile to API 





