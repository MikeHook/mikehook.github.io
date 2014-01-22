---
layout: post
title: "Dear Wordpress, you're dumped!"
date: 2014-01-15 16:38:27 +0000
comments: true
categories:
- Progress
published: true
---

<img src="http://imageshack.com/a/img827/7426/w5v5.png " class="alignleft" alttext="Octopress Logo"  />
There comes a time in a developers life (every couple of years it seems) when you are dissatisfied with your existing blogging platform. What had started as a beautiful and fulfilled relationship has decayed to the point where you barely interact. What once seemed like a cutting edge editing interface has become stayed and obtuse. Your eyes begin to wander, other blogging platforms seem so slick and modern in comparison, you wonder  "why can't I have some of that?"

But there is no need to fret, unlike [other breakups](http://www.sheknows.com/tags/celebrity-breakups) this change should be quite painless for all parties involved. So as January is the peak month for relationship change, there is no better time to ring the changes and introduce a new blogging platform!

## No more Batman and Robin
You may ask what my blog has to do with the dynamic crime fighting duo Batman and Robin? Well the new platform is based on [Jekyll](http://jekyllrb.com/), which is quite the opposite of dynamic. Using the power of Jekyll I can now generate all the static blog content (html, images, css) locally by running a couple of commands. Then I just push the changes up to [github](https://github.com/) and boom, the updates are published. 

I really liked the simplicity of this approach as a blog is pretty much all about the content, there are no interactive features or dynamically changing content that would require a processing engine. The functionality can be met with good old static html. There are also a bunch of other great advantages to this approach:

- Use of [Markdown](https://github.com/NeQuissimus/MarkdownByExample/wiki/MarkdownSyntax) for content editing 
- Faster page load times
- More tolerant to high loads (although I doubt the traffic on this particular blog will ever cause a problem!) 
- Free web hosting with [github pages](http://octopress.org/docs/deploying/github/)
- Scratches my developer itch for getting my hands dirty with the blogging platform 

There are some drawbacks to this approach such as a higher barrier to entry for editors, however as I'm the only editor that isn't a problem for me. 

## Wordpress meet Octopress
Having settled on a new platform I needed to extract the content from my existing wordpress blog and convert it into a format suitable for Octopress (ie. Markdown). Fortunately this is a fairly [well travelled road](http://import.jekyllrb.com/) and my posts were pretty basically formatted so the conversion process was pretty painless. The most important issues for me were to not lose formatting of the content and retain the same permalinks so existing google / bookmark links would still work.

I got the blog running pretty easily (as it runs through Ruby, which is already setup on my machine) however the default theme wasn't really doing it for me. Fortunately there are a bunch of [alternative themes](http://opthemes.com/) available that a pretty straightforward to apply. I opted to use the [greyshade theme](https://github.com/shashankmehta/greyshade) as a base and tweak the fonts / layout to suit my needs.

## Images / Comments
So at this point all the posts are in the blog nicely formatted but the images are still being served from Wordpress and even worse the comments have not been ported at all. 

I considered just dumping the images into the new blog platform itself, however I felt like there should be a better solution. Whilst my blog is pretty tiny at the moment the number of assets are bound to grow over time and I didn't want to cripple the blog hosting server itself with unnecessary load. Initially I tried uploading the images to [Picasa](http://picasa.google.com/), however it seems to require a desktop app to upload the images and refused to give me a proper URL for the image sources. A far better solution for me was [ImageShack](https://imageshack.com/), uploading is handled through a slick browser based interface and you can easily get direct links to the images. A nice bonus for the platform is built in support for image resizing, all you need to do is specify your required size in the image link.

The final part to integrate was the comments. This turned out really straightforward using [Disqus](http://disqus.com/). All I needed to do was sign up for a new account and point it at my existing wordpress blog to pull the comments down. You can enable Disqus in Octopress through the config.yml file. 
Then, provided your post links are maintained, when the blog is switched across the comments are also kept, simples!

## Conclusion
So there you have it, a freshly baked blogging site for your consumption pleasure, enjoy!



    
  

          




