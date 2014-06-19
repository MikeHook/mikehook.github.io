---
layout: post
title: "Mysql to Azure database migration"
date: 2014-06-19 16:57:57 +0100
comments: true
categories: 
- Hacking
published: true
---

<img src="http://imagizer.imageshack.us/v2/320x240q90/843/31nm.jpg" class="alignleft" alttext="Square peg - Round hole"  />

If you have read my previous post on [Umbraco to Azure migration](/2014/04/28/migrating-umbraco-4-mysql-to-azure-hosting/) the topic of this post will be of no surprise. 

I've inherited an application using a MySQL database which I'd like to host in Azure. However the hosting support for a MySQL database in Azure is expensive. So I have investigated migrating the DB into an SQL Azure supported format. 

##What about a VM?

That is a good question and one I didn't immediately consider. The Azure platform offers so many options that sometimes the most obvious can be missed. An easy way to keep the MySQL database without needing to fork out for the [ClearDB](https://www.cleardb.com/) service is to just spin up an Azure VM, install MySQL on the VM and host everything from there. 

This option offers a low overhead entry to hosting the site in Azure, with a lower cost than ClearDB. However I decided not to pursue it as it negates some of the advantages of cloud hosting, one of the aspects of Azure that is attractive is how streamlined you can make the deployment process, you can either download a publish profile and deploy directly from visual studio or hook it directly into your source control and deploy on check in. Also I must admit all the shiny monitoring graphs for Azure websites are pretty cool and give you great visibility of how your hosting is performing. Granted all these features could be achieved with a VM but not nearly so easily. So, onwards with the database migration challenge!

##Microsoft to the rescue! (nearly)

After a bit of research I decided to try using the [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/) to migrate the database. I was hoping this would automate all the tedious work and leave me just to press a few buttons, sit back and receive all the glory and admiration. Unfortunately it wasn't quite that simple as there are certain data types that just don't have a straight conversion between MySQL and SQL Server.

###How many ways can you say Yes and No?

At last, a simple question, surely we can all agree what 'Yes' and 'No' look like, after all its the basis for all digital computing! Unfortunately when it comes to technology nothing is quite that straight forward. In the world of MS SQL we have a [bit data type](http://msdn.microsoft.com/en-us/library/ms177603.aspx) for this function, 1 for Yes, 0 for No, simples. However the [MySQL bit data type](http://dev.mysql.com/doc/refman/5.7/en/bit-type.html) also takes a length parameter so it is only equivalent to MS SQL if the length is set to 1. 

To complicate things further MySQL actually advise in their [numeric types overview](http://dev.mysql.com/doc/refman/5.7/en/numeric-type-overview.html) that a TINYINT(1) data type should be used to represent a boolean value. However the actual values of this type can be anything from -128 to 127, pretty crazy huh! Unfortunately the database I am trying to migrate chose the MySQL recommended data type of TINYINT(1) and, quite understandably that is not supported by SSMA (SQL Server Migration Assistant) as a straight migration to bit. My solution for this was to craft a 'pre migration' script to manually convert all the MySQL booleans into a bit(1) data type, which could then be migrated by SSMA by adding a custom type mapping.

I also made a couple of other tweaks to the MySQL database before starting the conversion:

- Added primary keys to the identifying columns on the tables cmsmember, cmsstylesheet, cmsstylesheetproperty and umbracouserlogins
- Cleared out temporary data stored in the cmspreviewxml and umbracolog tables and run optimize on the tables to free up unused space 

I was then ready to fire up the SSMA tool and migrate the database to Azure. I won't go into details about this as there is already a [decent guide for using SSMA](http://blogs.msdn.com/b/ssma/archive/2011/02/07/mysql-to-sql-server-migration-how-to-use-ssma.aspx). 

##The Devil is in the detail

After the migration there was a final synchronisation step to carry out. I needed to manually check the data types for each of the columns in the Azure DB and update any that were not correct. I found out what the types were meant to be by [downloading the same umbraco version](http://our.umbraco.org/download) I was working with and comparing the types. I expect there is a tool that can be used to do this but the database wasn't particularly large so it didn't take too long to carry out manually. 

Most of the changes could be made directly to the Azure tables by just changing the datatype in the management tool. However a couple of columns proved more difficult, if they were being used as primary keys in azure there was no way to change them in place so instead I copied the data into a new table which had the correct schema, removed the old table and renamed the new table. Here is an example script for the cmstasktype table:

	EXECUTE sp_rename N'[PK_cmstasktype_ID]', N'[PK_cmstasktype_ID_old]',  'OBJECT'
	
	CREATE TABLE [Tempcmstasktype](
		[ID] [tinyint] NOT NULL,
		[ALIAS] [nvarchar](255) NOT NULL	
	 	CONSTRAINT [PK_cmstasktype_ID] PRIMARY KEY CLUSTERED 
		([ID] ASC
		)WITH (PAD_INDEX  = OFF, STATISTICS_NORECOMPUTE  = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS  = ON, ALLOW_PAGE_LOCKS  = ON) ) 
	GO

	Insert INTO [Tempcmstasktype] ([ID], [ALIAS])
		Select	[ID], [ALIAS]	From [cmstasktype]
	
	drop table [cmstasktype]
	EXECUTE sp_rename N'Tempcmstasktype', N'cmstasktype', 'OBJECT'
	
After ensuring all the data types matched I was able to fire up the azure website and hey presto everything functioned correctly! OK I admit it wasn't quite that smooth as I'd missed changing one column from an int to tinyint which broke the whole CMS admin UI but once I'd tracked that down everything worked fine, hooray!



