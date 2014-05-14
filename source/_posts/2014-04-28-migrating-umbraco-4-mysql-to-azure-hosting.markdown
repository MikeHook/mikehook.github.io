---
layout: post
title: "migrating umbraco 4 mysql to azure hosting"
date: 2014-04-28 09:58:09 +0100
comments: true
categories: 
- Hacking
published: false
---

##Why Azure 

I recently inherited an Umbraco version 4 site and need to setup web hosting for it. I decided that Azure would be a good option for hosting as it offers great benefits for future scalability and a good costing model. I also wanted to try it out and see what all the fuss is about!

There were a few steps to carry out to get the site up and running in Azure, so I've documented my experience here.

##First steps

- Obviously you need to sign up to a [Windows Azure account](http://azure.microsoft.com/). Its free to try but you do need a credit card. I guess this makes it nice and seamless for Microsoft to start charging you if you opt to continue past the 1 month trial! 
- Now login to the portal and go to Websites -> new -> custom create -> enter details, selecting a mysql database from the drop down

<img src="http://imageshack.com/a/img834/4281/jf2b.png" class="alignleft" alttext="Azure create website dialog"  />

##Migrating the database

- Click on the new website and go to the 'linked resources' tab. The MySql Database should be listed here
- Click the name to open ClearDB (the third party provider of MySql databases support for Windows Azure)
- Setup MySQL workbench to connect to ClearDB using an [SSL connection](https://github.com/CloudBees-community/tomcat-clickstack/wiki/ClearDB-::--MySQL-SSL-Connection-MySQL-Workbench). You may also need to install OpenSSL to enable generation of the rsa key. 
- Reduce the database size to fit in the 20MB size limit by [removing old document versions and preview / log files.](http://www.spyriadis.net/2012/07/umbraco-clear-old-document-versions-to-decrease-database-size-and-improve-performance/)

##Uploading the web site

- Download the publish profile file from your azure account. It is available on the dashboard page of your website, under the 'quick glance' menu
- Open the site in WebMatrix, press publish and browse to select the publish profile file you just downloaded.
- Web Matrix will now run a diff on the files and prompt you to upload any changes to Azure.
- In Azure go to the site configure tab and set the app settings for 'umbracoDbDSN' and any other connection string values to the ClearDB database connection string. If you are not sure what the connection string you can get it from the 'quick glance' menu.

I like this ability to set your production connection strings / app settings in Azure. This will be help enable me to open source the code without storing these secure settings in the config files themselves.

Hey presto, your site should be up and running!

##So, what's the verdict?

Is Azure as nice a hue as promised or is it more murky in practice? Well from my experience its all worked pretty much as expected so far. Its clear the Microsoft has invested some serious resources into the platform and unlike some of their other products (ahem Windows 8) the usability is excellent.

I think this is a decent work flow to initially get a site up and running. However in the long term I would want to hook up source control for the site to publish when merging to a branch. There is an option in Azure to 'Set up deployment from source control' so I will be investigating how this works next.

The main drawback I currently see with this setup is that the database has a limit on the number of connections. The free version has 4 which is fair enough, given it is just for trialling. However even the paid versions have low limits of 15, 30 or 40 for $10, $50 or $100 respectively. My feeling is that these limits may well cause problems when the site is running in production. I already hit the 4 connections limit just browsing the site myself and publishing a new page and it is bound to use more when real users are browsing at the same time.    

The SQL databases offered in Azure do not have this connections limit and would also be significantly cheaper in the long run, so I will be investigating the feasibility of migrating the Umbraco DB from MySQL into SQL.

