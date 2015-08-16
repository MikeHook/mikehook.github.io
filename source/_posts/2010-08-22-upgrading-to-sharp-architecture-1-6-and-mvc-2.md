---
layout: post
title: Upgrading to Sharp Architecture 1.6 and MVC 2
categories:
- Hacking
tags:
- MVC
- sharp architecture
status: publish
type: post
published: true
date: 2010-08-22 14:29:07
comments: true
meta:
  _wp_old_slug: ''
  _wpcom_is_markdown: '1'
  _elasticsearch_indexed_on: '2010-08-22 14:29:07'
---
After a good few months in hibernation the guilt of leaving my poor pet project half-finished has finally got to me!

I was also spurred on by several new (and not so new) software releases that I would like to try out:

<ul>
    <li><a href="http://devlicio.us/blogs/billy_mccafferty/archive/2010/08/19/s-arp-architecture-1-6-released.aspx">S#arp Architecture 1.6</a></li>
    <li><a href="http://weblogs.asp.net/scottgu/archive/2010/03/11/asp-net-mvc-2-released.aspx">MVC version 2</a></li>
    <li><a href="http://www.techfortesco.com/forum/index.php?topic=265.0">Tesco API Beta</a></li>
</ul>

The first step I needed to take was to upgrade the project to use the new release of Sharp Architecture and MVC 2. The Sharp Architecture wiki has   a <a href="http://wiki.sharparchitecture.net/1.5UpgradeGuide.ashx">decent upgrade guide</a> and I was able to get most of the way there by following this.   However there were a couple extra step I needed to take in my project:

<h3>Upgrade to Fluent NHibernate</h3>

I think my project may have been on an earlier version than that in the guide because I needed to upgrade the Data project to use Fluent NHibernate. The issues here were pretty obvious as there were several compilation errors. I found the easiest solution was to generate a new empty Sharp Architecture project from the 1.6 template, then copy across the amended files in the NHibernateMaps folder.

This got the application to compile OK, however there were a couple of differences in the configuration from my project.

<h3>Primary Key Convention</h3>

One of the data unit tests was failing when trying to access an entity using the <em>.Get(id)</em> method. This was strange as the entity was being loaded OK in the setup method. I was also getting an exception from the website when trying to add an entity to the database:

<blockquote><em>Invalid object name 'hibernate_unique_key'.</em></blockquote>

The problem was due to a change in the <em>PrimaryKeyConvention</em> class. The convention for generating primary keys has been changed from <em>Native()</em> to <em>HiLo("1000")</em>. So instead of the first record having a key of 0 it would now try to use 1000. Changing this setting back to <em>Native()</em> fixed the problem straight away.

<h3>Cascade Conventions</h3>

The other issue I encountered occurred when trying to add a new ingredient to a recipe:

<blockquote><em>Cannot insert the value NULL into column 'IngredientFk', table  'recipebook.dbo.RecipeIngredientDrafts'; column does not allow nulls. INSERT  fails.
The statement has been terminated.</em></blockquote>

This error pointed to a problem with NHibernate not saving changes to associated entities; it was trying to save a new <em>RecipeIngredientDraft</em> record without first saving the new <em>Ingredient </em>record. This issue could be resolved by adding a call to <em>instance.Cascade.SaveUpdate() </em>in the <em>ReferenceConvention </em>and <em>HasManyConvention </em>classes.

After fixing these issues the application worked just fine :)