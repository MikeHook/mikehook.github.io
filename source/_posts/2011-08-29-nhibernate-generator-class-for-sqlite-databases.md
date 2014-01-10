---
layout: post
title: Nhibernate generator class for SQLite databases
categories:
- Hacking
tags:
- FootyLinks
- NHibernate
status: publish
type: post
published: true
date: 2011-08-29 14:40:26
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-08-29 14:40:26'
  _wpcom_is_markdown: '1'
---
I've been developing a mobile app for Android which needs to query data in an SQLite database. So the first task I needed to get done was to import data into the SQLite database. As I'm more used to writing .NET applications which use the NHibernate ORM I decided to write an import program on this technology stack.

For my first attempt at the import program I just wanted to focus on the import logic, so choose to use a 'known working' NHibernate configuration with a MS SQL 2005 database. While this worked fine for me it did mean that I had to manually copy all of the data from the MS SQL database into the SQLite database before it could be used in the Android app. This was quite laborious so when I needed to modify the importer at a later date I decided to change the NHibernate configuration to point directly to the SQLite database instead. This was the updated config file used:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">&lt;hibernate-configuration  xmlns="urn:nhibernate-configuration-2.2" &gt;
    &lt;session-factory&gt;
        &lt;property name="connection.provider"&gt;NHibernate.Connection.DriverConnectionProvider&lt;/property&gt;
        &lt;property name="connection.driver_class"&gt;NHibernate.Driver.SQLiteDriver&lt;/property&gt;
        &lt;property name="connection.connection_string_name"&gt;FootyLinks&lt;/property&gt;
        &lt;property name="dialect"&gt;NHibernate.Dialect.SQLiteDialect&lt;/property&gt;
        &lt;property name="query.substitutions"&gt;true=1;false=0&lt;/property&gt;
    &lt;/session-factory&gt;
&lt;/hibernate-configuration&gt;</pre>

And this is an example of the connection string format:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">&lt;configuration&gt;
    &lt;connectionStrings&gt;
        &lt;add name="FootyLinks" connectionString="Data Source=C:\path_to_the_SQLite_database_file;Version=3"/&gt;
    &lt;/connectionStrings&gt;
&lt;/configuration&gt;</pre>

This configuration worked OK and the import program could connect to the SQLite database, however I hit a problem when trying to save new records to the database. This was the inner exception reported by NHibernate:

<blockquote>Abort due to constraint violation Club.Id may not be NULL</blockquote>

The problem was that the import program was using the default generator class of <em>identity  </em>for in the NHibernate entity mappings. So NHibernate was expecting the database to generate the identity keys for the new records being saved to the database, however SQLite does not appear to support this. After some reading up on the options available for the <a href="http://nhforge.org/doc/nh/en/index.html#mapping-declaration-id-generator">NHibernate generator setting</a>  I decided to try the  <em>native  </em>setting. As this should select the implementation supported by the database. However this didn't work either, I think because, of all the options <em>native</em> can select, none are actually supported by SQLite. So my next preferred selection was to use <em>increment,</em>  this setting comes with a disclaimer that it should not be used in a cluster, which is absolutely fine as its very unlikely that I will ever be deploy the importer to a load balanced environment.

The <em>increment</em> setting worked fine with the SQLite database and should streamline my workflow nicely for updating the app data .
