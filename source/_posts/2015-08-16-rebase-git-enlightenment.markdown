---
layout: post
title: Rebase your way to Git enlightenment 
comments: true
categories: 
- Tips and Tricks
date: 2015-08-16 18:07:49 +01:00
published: true
---

<img src="http://i49.photobucket.com/albums/f299/hookmike/Pat-Morita_Karate_Kid_200_zpscufzef0b.jpg" class="alignleft" title="Pat Morita" /> 

The path to Git enlightenment can be a long one for a developer used to centralized source control such as SVN.

The first signs of trouble usually occur when trying to apply changes from one branch of code to another. Some file change conflicts are likely if the same source file has been changed on both branches. The source control system has no way to know which changes should be kept so it will quite rightly ask the developer to choose, however this can cause some difficulty if the changes have been made by someone else who may not even be around to ask what has happened.

I have a feeling that the great Mr Miyagi would have loved using Git, as patience and dedication to the ways of [DVCS](https://en.wikipedia.org/wiki/Distributed_revision_control) are well rewarded in time. Indeed, dealing with branches is one situation where patience is important to avoid introducing regression bugs.

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Kesuke_Miyagi Kesuke Miyagi %}
Patience, young grasshopper.
{% endblockquote %}

There are a couple of different techniques which can be used to bring the changes on two branches together and the Atlassian site has a great write up of these in it's [merging vs rebasing article](https://www.atlassian.com/git/tutorials/merging-vs-rebasing/). The article describes the following scenario, you have created a **feature** branch from the **master** and made some commits to your branch. Someone else has since made some changes to the master branch which you now need to include in your branch, to get these changes you can either merge or rebase. 

My personal preference is to use the rebase command where possible for one key reason, **merge conflicts are easier to resolve!** The reason for this is that when rebasing you apply **your changes** on top of the master changes. This is different to a merge which will apply the master changes on top of your feature changes. So effectively any conflicts which occur will be due to changes made by yourself instead of someone else. My memory may not be great but I stand roughly 100% more chance of remembering something I've done as opposed to something someone else has done!

Hopefully you can see the benefits of this and are thinking, 'great I'm going to give that a go!' But before you rush off a few words of warning, there is potential for things to go horribly wrong.

{% blockquote Wikipedia https://en.wikipedia.org/wiki/Kesuke_Miyagi Kesuke Miyagi %}
First learn stand, then learn fly
{% endblockquote %}

The trouble comes if there are 2 people working on the feature branch who have both pushed changes to a remote repository. If you haven't fetched their changes before you push the rebased feature branch their changes will get overwritten and lost forever! That would be bad but Git does try to help you out by blocking the push, actually the only way you can push the rebased branch is by passing an extra parameter to **force** the push. So this acts as a nice reminder to think about what you are doing, only tick that 'force push' box if you are sure your branch is fully up to date. Actually I'd never use rebase if I had any doubt that anyone else might be working on the same branch as me.

A final note about rebase, it may be difficult to carry out with some Git GUI tools. Some of my colleagues like using [Source Tree](https://www.sourcetreeapp.com/) and I'll agree it does look nice, however it doesn't support this workflow well at all. After starting a rebase you get left in a weird limbo state and have to keep re-requesting to rebase the branches after each and every commit. Then once the rebase is complete there is no way to force the push through the UI, you have to drop down to the command line to complete the action. I'd recommend giving [Git Extensions](https://code.google.com/p/gitextensions/), the reliable Ford Mondeo of Git GUIs, a go for this scenario, it work much more smoothly.   






