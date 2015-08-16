---
layout: post
title: ! 'Developing Android Apps: Connect to an existing SQLite database'
categories:
- Hacking
tags:
- Android
- FootyLinks
status: publish
type: post
published: true
date: 2011-12-30 14:43:17
comments: true
meta:
  _wpas_done_linkedin: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"12339140";s:7:"blog_id";s:8:"11998060";s:9:"mod_stamp";s:19:"2011-12-30
    14:43:17";}
  publicize_results: a:1:{s:7:"twitter";a:1:{i:33610861;a:2:{s:7:"user_id";s:12:"michael_hook";s:7:"post_id";s:18:"152761703438893057";}}}
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-12-30 14:43:17'
  _wpcom_is_markdown: '1'
---
This post was drafted some time ago, so some of the details are lost in the mists of time. But I'll provide links to the source so anyone interested in doing something similar can start from there.

The android app I'm developing needs to be able to connect to an existing SQLite database, there are not many examples of how to achieve this in the Android tutorials, however I was able to find some guidance in a post from another Android developer on the <a href="http://www.reigndesign.com/blog/using-your-own-sqlite-database-in-android-applications/">reigndesign blog</a>

The key is that the database needs to be copied internally to the system directory on the device running the application before it can be accessed. This doesn't seem particularly efficient but I'm sure Android had their reasons! My modified version of the code to do this is available from my github repository here:

<a href="https://github.com/MikeHook/FootyLinks/blob/master/AndroidApp/src/mhook/FootyLinks/Data/FootyLinksSQLLiteHelper.java">FootyLinksSQLLiteHelper.java</a>

I've also encountered a couple of other snags since then while working with the SQLite database.

<h3>No password support</h3>

Passwords do not appear to be supported at all for the Android applications. I'd automatically added password protection to the database when creating it and it took me a while to figure out why eclipse was refusing to read it!

<h3>Manually delete after schema changes</h3>

I've added some additional fields to the database over time. However initially they were not being copied into the version of the database in my device emulator. I found it was necessary to browse the emulator files themselves, using the DDMS perspective in Eclipse, and delete the old database file directly in the emulator file system.