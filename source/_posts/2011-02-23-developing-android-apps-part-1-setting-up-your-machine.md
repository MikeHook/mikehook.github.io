---
layout: post
title: ! 'Developing Android Apps - Part 1: Setting up your machine'
categories:
- Hacking
tags:
- Android
- Java
status: publish
type: post
published: true
date: 2011-02-23 19:48:46
comments: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-02-23 19:48:46'
  _wpcom_is_markdown: '1'
---
This is the first in a series of posts I'll be making about developing Android applications. Hopefully there will be some useful tips in this series that will help other developers to get their apps off the ground.

The first step is to get your PC setup with all the required software for writing the apps. There is a pretty good guide <a href="http://developer.android.com/sdk/index.html">setting up the Android SDK</a> on the Android developer site so I won't go into too much detail. However, I think their steps are in the wrong order so I've amended them below (you should install the Android platforms before the ADT plugin):

<ol>
    <li>Prepare your development computer and ensure it meets the system requirements.
<ul>
    <li>You need to install the Java Development kit here, if you just run their SDK installer it provides a link to the JDK.</li>
</ul>
</li>
    <li>Install the SDK starter package from the table above. (If you're on Windows, download the installer for help with the initial setup.)</li>
    <li>Add Android platforms and other components to your SDK.
<ul>
    <li>I only added the 2 latest SDK platforms along with their samples, I doubt you would need the older ones unless you have to develop for some legacy device.</li>
</ul>
</li>
    <li>If you don't already have it,  <a href="http://www.eclipse.org/downloads/">download Eclipse</a> and install it
<ul>
    <li>While the platforms are downloading you can grab a copy of eclipse. It doesn't need installing as such you can just extract to your hard drive.</li>
</ul>
</li>
    <li>Install the <a href="http://developer.android.com/sdk/eclipse-adt.html">ADT Plugin for Eclipse</a>.
<ul>
    <li>After Eclipse has extracted and your android platforms are added, you can hook the Android SDK into Eclipse by installing the ADT plugin.</li>
</ul>
</li>
    <li>Explore the contents of the Android SDK (optional).</li>
</ol>

Once all the above are done you should be ready to start developing. A good place to start is with the <a href="http://developer.android.com/resources/tutorials/hello-world.html">hello world tutorial</a>, that takes you through setting up an Android virtual machine on    your computer and getting the application to run on it.

I also wanted to see an app actually running on my phone so followed the <a href="http://developer.android.com/guide/developing/device.html">developing on a device guide</a> to get this up and running. I did hit a snag here with the USB drivers, my phone is an HTC desire and there is no driver to download from the HTC site. However if you download the HTC sync software then plug your phone into the USB it should install the driver for you. I had to restart the phone afterwards before it would bring up the menu to select the connection type, eventually the sync software sorted itself out and recognised the phone.

When you select to run the application in Eclipse, your phone should then appear under the 'Choose a running Android device' section. The app can be installed on the phone then selected from the usual apps menu.