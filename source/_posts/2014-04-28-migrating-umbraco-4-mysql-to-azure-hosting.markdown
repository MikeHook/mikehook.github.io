---
layout: post
title: "migrating umbraco 4 mysql to azure hosting"
date: 2014-04-28 09:58:09 +0100
comments: true
categories: 
---

##Intro

Some intro here

##First steps

Things here

- Sign up to windows azure
- Go websites -> new -> custom create -> enter details, selecting a mysql database from the drop down

*insert pic

##Migrating the database

- Click on the new website and go to the 'linked resources' tab. The MySql Database should be listed here
- Click the name to open ClearDB (the third party provider of mysql databases support for Windows Azure)
- Setup MySQL workbench to connect to ClearDB, [this article](https://github.com/CloudBees-community/tomcat-clickstack/wiki/ClearDB-::--MySQL-SSL-Connection-MySQL-Workbench) is useful!  





