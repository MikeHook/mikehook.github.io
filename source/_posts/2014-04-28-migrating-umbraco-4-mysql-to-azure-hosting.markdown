---
layout: post
title: "migrating umbraco 4 mysql to azure hosting"
date: 2014-04-28 09:58:09 +0100
comments: true
categories: 
- Hacking
published: false
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
- Setup MySQL workbench to connect to ClearDB using an [SSL connection](https://github.com/CloudBees-community/tomcat-clickstack/wiki/ClearDB-::--MySQL-SSL-Connection-MySQL-Workbench). Also may need to install OpenSSL to enable generation of the rsa key. 
- Reduce the database size to fit in the 20MB size limit by [removing old document versions and preview / log files.](http://www.spyriadis.net/2012/07/umbraco-clear-old-document-versions-to-decrease-database-size-and-improve-performance/)

##Uploading the web site

- Download the publish profile file from your azure account. It is available on the dashboard page of your website, under the 'quick glance' menu
- Open the site in WebMatrix, press publish and browse to the publish profile file you just downloaded.
- Web Matrix will now run a diff on the files and prompt you to upload any changes to Azure.
- In Azure go to the site configure tab and set the app settings for 'umbracoDbDSN' and any other connection string values to the ClearDB database connection string. If you are not sure what the connection string you can get it from the 'quick glance' menu.

I like this ability to set your production connection strings / app settings in Azure. This will be help enable me to open source the code without storing these secure settings in the config files themselves.

Hey presto, your site should be up and running!

##So, what's the verdict?



I think this is a decent work flow to initially get a site up and running. However in the long term I would want to hook up source control for the site to publish when merging to a branch. There is an option in Azure to 'Set up deployment from source control' so I will be investigating how this works next.

The main drawback I currently see with this setup is that the database has a limit on the number of connections. The free version has 4 which is fair enough, given it is just for trialing. However even the paid versions have low limits of 15, 30 or 40 for $10, $50 or $100 respectively. My feeling is that these limits may well cause problems when the site is running in production. I already hit the 4 connections limit just browsing the site myself and publishing a new page and it is bound to use more when real users are browsing at the same time.    

The SQL databases offered in Azure do not have this connections limit so I will be investigating the feasibility of migrating the Umbraco DB from MySQL into SQL.





