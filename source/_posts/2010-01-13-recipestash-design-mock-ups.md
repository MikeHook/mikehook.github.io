---
layout: post
title: Recipestash design mock-ups
categories:
- Hacking
tags:
- mock-ups
- recipestash
status: publish
type: post
published: true
date: 2010-01-13 19:51:00
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '7018562398869224176'
  _elasticsearch_indexed_on: '2010-01-13 19:51:00'
  _wpcom_is_markdown: '1'
---
The recipestash website is flush with a shiny new domain name, however it was sadly lacking in any kind of visual design. Up to now I   ve been working with a generic style sheet downloaded from <a href="http://www.freecsstemplates.org/">free CSS templates</a> however I think it is poorly suited to the site. So I   ve put some time into making some mock-ups in Photoshop. </p>

I   ve identified the site homepage, main search page and    Add a recipe    pages as the main areas to mock up. The remaining sections can be investigated later. The design mock-ups for each of the pages are shown below (full size versions can be viewed in the <a href="http://picasaweb.google.com/hook.mike/BakingWebsites?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite">Picasa web album</a>).

<h4><a href="http://picasaweb.google.com/lh/photo/SMxU9grhwyO-94vtTC6wlw?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=directlink">Home</a></h4>

<table style="width:auto;"><tbody>     <tr>       <td><a href="http://picasaweb.google.com/lh/photo/SMxU9grhwyO-94vtTC6wlw?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite"><img src="http://lh4.ggpht.com/_5hQeqbPSrks/S0oq83g-3mI/AAAAAAAAACU/Itr3eLIfQac/s400/home-5.png" /></a></td>     </tr>      <tr>       <td style="text-align:right;font-family:arial, sans-serif;font-size:11px;">From <a href="http://picasaweb.google.com/hook.mike/BakingWebsites?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite">Baking Websites</a></td>     </tr>   </tbody></table>

I   ve gone for a minimal design on the homepage, the header will be used across all pages on the site and includes the site name, main search bar and login controls for the current user. The main content on the homepage highlights each of the four sections on the site which will be used most often. I   ve chosen a different colour for each of the main sections, hopefully this will give users a visual feel of where they are on the site. The four icons on the left will be displayed on all of the pages and be used as the main means of navigation. I   ll be looking to use some jQuery to display the section names when each of the icons is hovered over. If javascript is not available in the browser then a simple navigation menu will be displayed in the header.

<h4><a href="http://picasaweb.google.com/lh/photo/rGAahgUi9lxqrOS69MuHoQ?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=directlink">Browse recipes</a></h4>

<table style="width:auto;"><tbody>     <tr>       <td><a href="http://picasaweb.google.com/lh/photo/rGAahgUi9lxqrOS69MuHoQ?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite"><img src="http://lh6.ggpht.com/_5hQeqbPSrks/S04japbJMsI/AAAAAAAAAC0/sW00NCbkFyc/s400/browse-2.png" /></a></td>     </tr>      <tr>       <td style="text-align:right;font-family:arial, sans-serif;font-size:11px;">From <a href="http://picasaweb.google.com/hook.mike/BakingWebsites?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite">Baking Websites</a></td>     </tr>   </tbody></table>

The    Browse recipes    page can be reached from either the navigation menu or the search bar. The main features of the page are:

<h5>Search keywords</h5>

If the page has been reached from a search the top section displays the keywords which have been searched for. Each of these can be removed from the search by pressing the    x    icon next to the keyword.

<h5>Filters </h5>

Each of the filters are a simple checkbox, which will cause the results to display only those matching the filters. I may include an    apply filters    button for accessibility as performing a postback on the checkbox selection would most likely require some javascript to operate.

<h5>List of recipes </h5>

The list of recipes will include summary information for each of the recipes, as shown in the mock-up above. The ordering of the recipes can be controlled by clicking on the column headings. I will look to colour one of the headings to indicate which is currently being used to order the results.

<h5>More recipes link </h5>

Clicking the link will cause more recipes to be loaded to the bottom of the search results.&#160;&#160; 

<h4><a href="http://picasaweb.google.com/lh/photo/cC7X3M6u_Jo1M_AuXDawQw?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=directlink">Add a recipe</a></h4>



<table style="width:auto;"><tbody>     <tr>       <td><a href="http://picasaweb.google.com/lh/photo/cC7X3M6u_Jo1M_AuXDawQw?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite"><img src="http://lh4.ggpht.com/_5hQeqbPSrks/S0pHr70y72I/AAAAAAAAACw/f6YE3GYZR-0/s400/add-recipe-1.png" /></a></td>     </tr>      <tr>       <td style="text-align:right;font-family:arial, sans-serif;font-size:11px;">From <a href="http://picasaweb.google.com/hook.mike/BakingWebsites?authkey=Gv1sRgCL7W-eqGn5fIcA&amp;feat=embedwebsite">Baking Websites</a></td>     </tr>   </tbody></table>

The functionality of the    Add a recipe    page is already largely complete. The above styling will need to be applied to the pages to format the form as displayed above. It may be tricky to apply the styling shown to the text boxes but there are other sites using similar effects so I can probably find some hints for the required CSS techniques from them!

&lt;

p&gt;Next up I will be looking to generate some HTML and CSS to match the mock-ups which can be used in the website views.