---
layout: post
title: Become a Git contortionist with Stash
categories:
- Tips and Tricks
tags:
- git
- Testing
status: publish
type: post
published: true
date: 2013-01-18 19:16:05
comments: true
meta:
  _wpcom_is_markdown: '1'
  _wpas_done_327858: '1'
  publicize_twitter_user: michael_hook
  _wpas_done_327859: '1'
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:33610861;b:1;}}
  publicize_reach: a:2:{s:7:"twitter";a:1:{i:327859;i:156;}s:2:"wp";a:1:{i:0;i:1;}}
  _wpas_skip_327859: '1'
  _wpas_skip_327858: '1'
  _elasticsearch_indexed_on: '2013-01-18 19:16:05'
---
<img class="size-medium wp-image-364 alignright" alt="Annie the Contortionist" src="http://imageshack.com/a/img856/5731/jyt7.jpg" width="300" />One of the great aspects of the Git source control system is its flexibility. Almost any sticky situation you may encounter  as a developer working with source control can be   solved with Git.

Take this scenario, you're halfway through writing a class for a new feature when you get a call.. the client has found a show stopping bug in the application and it needs to be fixed right away. To be able to fix the bug you will need to switch onto the live code branch but you can't go switching branches with a load of uncommitted changes.  Now you don't want to commit your code as its half done and won't even compile right now, but you don't want to lose the changes either... <a href="http://git-scm.com/book/en/Git-Tools-Stashing"><em>git stash</em></a> to the rescue!

When you issue the git stash command all your changes will be committed to a stack. Leaving you with no changes on your current working branch and free to branch where ever you want. So now you can switch to the live branch and do your bug fixing work. Once that is done you can simply move back to the feature branch and <em>apply</em> the stashed changes. Hey presto, you are back where you started with your uncommitted half complete work!

If you really get into lots of parallel work you can stash multiple times, just be sure to remember that it is a stack and your last changes you have stashed will come back off stack first.