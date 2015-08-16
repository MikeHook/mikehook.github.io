---
layout: post
title: EPiServer 6 developer exam tips
categories:
- Tips and Tricks
tags:
- EPiServer
status: publish
type: post
published: true
date: 2011-01-23 17:35:34
comments: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-01-23 17:35:34'
  _wpcom_is_markdown: '1'
---
Last Monday I made the trip to London to take the EPiServer CMS certification exam (version 6). I passed but it was quite a tricky exam so I thought I'd post some tips that may be of help to others taking the test.

The questions are randomised so you won   t get asked the same as me but the areas will probably be similar. There were 70 multiple choice questions split into four sections. I can't remember the exact breakdown but these numbers are close:

<h3>Installation / deployment (~10 questions)</h3>

These questions are not particularly difficult but if, like myself, you haven't needed to use the features much previously you might get stuck.

<ul>
    <li>Deployment center     make sure you know everything you can do from here</li>
    <li>Initialization module     What is it, when does it run etc (this is new to version 6, I missed the tech note and wished I'd read it)</li>
    <li>Multi-Site installations</li>
    <li>Upgrading to v6     What versions can you upgrade from, what is backed up / updated.</li>
</ul>

<h3>General Product Knowledge (~15 questions)</h3>

If you are familiar with the system and its features there is not much to worry about here

<ul>
    <li><span style="font-weight:normal;font-size:13px;">Load balancing</span></li>
    <li>Page type &amp; page template concepts</li>
    <li>XForms - How to make one, how it works</li>
    <li>Online center and gadgets</li>
</ul>

<h3>Standard Development (~25 questions)</h3>

Again, if you have developed a couple of sites then most of these are not too bad. However the customisation questions will be sticking points unless you are familiar with the tech notes.

<ul>
    <li>Wiring up EPiServer / .NET web controls</li>
    <li>Wiring up master pages</li>
    <li>Custom membership provider</li>
    <li>Custom virtual path provider</li>
    <li>Globalization     What language is used in various scenarios</li>
</ul>

<h3>Advanced Development (~20 questions)</h3>

These questions are really tricky, and may involve quite a bit of guess work!

<ul>
    <li>Mirroring     How is it setup / configured, what is mirrored</li>
    <li>Scheduler service     What can and can   t be run</li>
    <li>Dynamic data store</li>
    <li>Page Objects (I missed this one completely)</li>
    <li>Filters     adding code to a filter web control</li>
    <li>Quick publishing configuration</li>
    <li>Plugins / Modules     Installing</li>
</ul>

<h3>Preparing for the exam</h3>

To help pass I   d recommend in order of priority:

<ol>
    <li>Install the example site and run everything you can in the deployment center</li>
    <li>Try everything out in the CMS Online center (the Start page and menu bar) editor and admin sections while reading through the editor and admin manuals (particularly the admin manual)</li>
    <li>Read all the tech notes and try to setup what they are describing. I wouldn't worry about spending too much time on each though as long as you understand the concepts.</li>
</ol>

There are quite a lot of resources available online and it can be a bit overwhelming. But don't despair, Jonathan's handy  <a href="http://www.jonathansewell.co.uk/index.php/2010/12/06/episerver-6-exam-bookmarks/">exam bookmarks post</a> is a good place to start!