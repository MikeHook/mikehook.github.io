---
layout: post
title: Investigating an NHibernate 'stale state' exception
categories:
- Tips and Tricks
tags:
- NHibernate
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-01-16 18:01:36'
  _wpcom_is_markdown: '1'
---
I recently needed to investigate and fix this rather unpleasant sounding issue. It was  occurring, seemingly at random, on a live web service but could not be replicated in tests.  Occasionally the web service was throwing the exception when we made a call to delete a list of records from the database.

The web service uses the NHibernate object relational mapper (ORM) tool to map our c# classes (domain objects ) to database tables. NHibernate was throwing this exception when we tried to delete the records:

<blockquote>NHibernate.StaleStateException: Unexpected row count: 0; expected: 1</blockquote>

<h3>What is a stale state?</h3>

This issue was not one we had seen before and had us scratching our heads for a while. The first step was to understand what the exception itself meant.

After some reading up in the <a href="http://nhforge.org/doc/nh/en/index.html">NHibernate documentation</a> it was more clear what the issue was.  NHibernate was complaining that the in-memory model of the database data had become stale, or in other words out of date.  So when NHibernate tried to persist the requested deletion to the database it was finding that it could not complete the operation.

<h3>Why was our state stale?</h3>

So, all well and good, we knew what the stale state was. However, it was rather more difficult to pin down why it was happening, especially because we were unable to reproduce the error during testing.

It was only examining the event logs for the web service which enabled us to understand why this was happening.  The event logs showed that 2 separate instances of the web service were being called almost simultaneously on separate threads.  Both of those instances were basically calling the same delete operation on the same list of records.

So the first instance would succeed in deleting the records, but the second instance would then also try to delete the records.  Of course the second instance would fail, as there were not longer any records to delete.

<h3>Preventing the stale state (odour  eaters?)</h3>

Once we understood why the exception was occurring it was a straight forward re-factoring exercise to prevent it in future.  In our case the code could be updated so that the delete operation never needed to be called in the first place, so this was an effective fix.

The detailed event logging on the web service proved invaluable in solving this issue, without this it probably would of taken much longer to figure out what was happening!
