---
layout: post
title: "New year, New blogging platform"
date: 2014-01-15 16:38:27 +0000
comments: true
categories: progress
published: false
---

<img src="http://imageshack.com/a/img827/7426/w5v5.png " class="alignleft" alttext="Octopress Logo"  />

## Intro

blah blah blah 

## No more Batman and Robin

You may ask what does my blog have to do with the dynamic crime fighting duo Batman and Robin? Well the new platform is based on [Jekyll](http://jekyllrb.com/), which is quite the opposite of dynamic. Using the power of Jekyll I can now generate all the static blog content (html, images, css) locally by running a couple of commands. Then I just push the changes up to github and boom, the updates are published. 

I really liked the simplicity of this approach as a blog is pretty much all about the content, there are no interactive features or dynamically changing content that would require a processing engine. The functionality can be met with good old static html. There are also a bunch of other great advantages to this approach:

- Use of [Markdown](https://github.com/NeQuissimus/MarkdownByExample/wiki/MarkdownSyntax) for content editing 
- Faster page load times
- More tolerant to high loads (although I doubt the traffic on this particular blog will ever cause a problem!) 
- Free web hosting with [github pages](http://octopress.org/docs/deploying/github/)
- Scratches my developer itch for getting my hands dirty with the blogging platform 

There are some drawbacks to this approach such as a higher barrier to entry for editors, however as I'm the only editor that isn't a problem for me. 

## Wordpress meet Octopress

Having settled on a new platform I needed to extract the content from my existing wordpress blog and convert it into a format suitable for Octopress (ie. Markdown). Fortunately this is a fairly [well travelled road](http://import.jekyllrb.com/) and my posts were pretty basically formatted so the conversion process was pretty painless. The most important issues for me was to not lose formatting of the content and retain the same permalinks so existing google / bookmark links would still work.

I got the blog running pretty easily (as it runs through Ruby, which is already setup on my machine) however the default theme wasn't really doing it for me. Fortunately there are a bunch of [alternative themes](http://opthemes.com/) available that a pretty straightforward to apply. I opted for a customised version of the [greyshade theme](https://github.com/shashankmehta/greyshade).

## Images / Comments bit

## Conclusion
So there you have it, a freshly baked blogging site for your consumption pleasure, enjoy!



    
  

          




