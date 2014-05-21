---
layout: post
title: "Mysql to Azure database migration"
date: 2014-05-21 16:57:57 +0100
comments: true
categories: 
- Hacking
published: false
---

<img src="http://imageshack.com/a/img843/5127/31nm.jpg" class="alignleft" alttext="Square peg - Round hole"  />

If you have read my last post on [Umbraco to Azure migration](/2014/04/28/migrating-umbraco-4-mysql-to-azure-hosting/) the topic of this post will be of no surprise. I've inherited an application using a MySQL database which I'd like to host in Azure. However the hosting support for a MySQL database in Azure is expensive. So I have investigated migrating the DB into an SQL Azure supported format. 

##What about a VM?

That is a good question and one I didn't immediately consider. The Azure platform offers so many options that sometimes the most obvious can be missed. An easy way to keep the MySQL database without needing to fork out for the [ClearDB](https://www.cleardb.com/) service is to just spin up an Azure VM, install MySQL on the VM and host everything from there. 

This option offers a low overhead entry to hosting the site in Azure, with a lower cost than ClearDB. However I decided not to pursue it as it negates most of the advantages of cloud hosting, the site may as well be on a traditional hosting platform as there is no easy way (as far as I'm aware) to scale out a VM. Also it would be difficult to setup a deployment process directly from source control into the VM. So, onwards with the database migration challenge!

##Microsoft to the rescue! (nearly)

Blah blah, some stuff about [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/)

##How many ways can you say Yes and No?

At last, a simple question, surely we can all agree what 'Yes' and 'No' look like, after all its the basis for all computing! Unfortunately when it comes to technology nothing is quite that straight forward. In the world of MS SQL we have a [bit data type](http://msdn.microsoft.com/en-us/library/ms177603.aspx) for this function, 1 for Yes, 0 for No, simples. However the [MySQL bit data type](http://dev.mysql.com/doc/refman/5.7/en/bit-type.html) also takes a length parameter so it is only equivalent to MS SQL if the length is set to 1. 

To complicate things further MySQL actually advise in their [numeric types overview](http://dev.mysql.com/doc/refman/5.7/en/numeric-type-overview.html) that a TINYINT(1) data type should be used to represent a boolean value. However the actual values of this type can be anything from -128 to 127, pretty crazy huh! Unfortunately the database I am trying to migrate chose the MySQL recommended data type of TINYINT(1) and, quite understandably that is not supported by SSMA (SQL Server Migration Assistant) as a straight migration to bit. My solution for this was to craft a 'pre migration' script to manually convert all the MySQL booleans into a bit(1) data type, which could then be migrated by SSMA by adding a custom type mapping.             

