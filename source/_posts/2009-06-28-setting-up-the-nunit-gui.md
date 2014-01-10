---
layout: post
title: Setting up the NUnit GUI
categories:
- Testing
tags:
- NUnit
- TDD
- TestDriven.Net
- Unit Testing
status: publish
type: post
published: true
date: 2009-06-28 12:30:00
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '1027098612660756480'
  _elasticsearch_indexed_on: '2009-06-28 12:30:00'
  _wpcom_is_markdown: '1'
---
I've setup the project using the default 'S#arp architecture Application' template. I won't go into detail about the steps taken to do this as it is all covered in the Sharp Architecture documentation. However I did hit a snag setting up the <a href="http://www.nunit.org/index.php">NUnit</a> project so I'll expand on that in this post.

The Sharp Architecture template is setup for <a href="http://www.agiledata.org/essays/tdd.html">TDD</a> (Test Driven Design) and this is the approach I will be taking to the development. Having installed <a href="http://www.testdriven.net/">TestDriven.Net</a>, the tests can be run through visual studio, however I prefer the interface of the NUnit GUI. After each test run the results are displayed as a set of pleasing green ticks and/or nasty red crosses. Once you have a nice set of green ticks it becomes a bit of a fixation to keep it that way!

The unit tests for the visual studio project can be easily loaded into the NUnit GUI. All you need to do is go to File -&gt; Open Project and navigate to the bin folder of the test project. Opening the compiled assembly (dll) of the test project will load all the unit tests into the GUI. Hitting the run button returns a nice list of green ticks as expected. However after saving the NUnit test project something alarming happens on the next test run. The standard Sharp Architecture Data tests all fail and nasty set of red ticks appear!

The following error message is reported:
<br />

<blockquote style="font-style:italic;">
SharpArch.Core.PreconditionException : Please add an AppSetting to your app.config for 'nhibernate.mapping.assembly.' This setting takes a comma delimited list of assemblies containing NHibernate mapping files. Including '.dll' at the end of each is optional.<br />
</blockquote>

The error is pointing to a missing setting in the app.config file, however on checking the file the setting is present. Also the tests worked just fine before saving the NUnit project so why would they suddenly start to fail?

The cause of the test failures appears to have been saving the NUnit project, this pointed to some settings changes which NUnit may have made during the save. Taking a look at the NUnit project settings the possible cause of the problem became clear. There are a couple of directory and file paths in the settings, of particular interest are a 'Configuration File Name' and 'ApplicationBase' settings.

<span style="font-size:100%;font-weight:bold;">Configuration File Name</span>
When the test project assembly is first loaded this setting has a value of <span style="font-style:italic;">RecipeBook.Tests.dll.config</span>, however after saving the NUnit project the <span style="font-style:italic;">.dll </span>part of the filename disappears. So NUnit is no longer able to find the config file and read the settings from it.

<span style="font-weight:bold;">ApplicationBase</span>
The ApplicationBase setting remains blank, however if you have saved the NUnit project in a different directory to the test project assembly you will need to change this. The path needs to be the relative path from your test project base to the test assembly location.

Below is an image of the settings for my project:

<a href="http://bakingwebsites.files.wordpress.com/2009/06/nunitprojecteditor.jpg"><img alt="" border="0" src="http://bakingwebsites.files.wordpress.com/2009/06/nunitprojecteditor.jpg?w=300" /></a>
After updating these settings NUnit was able to read the settings for the config file and the GUI happily reported a lovely set of green ticks :)<br />
