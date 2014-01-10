---
layout: post
title: You have 20 seconds to comply
categories:
- Hacking
tags:
- CSS
- Html
- recipestash
- Validation
status: publish
type: post
published: true
date: 2010-02-12 09:00:00
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '8328022709446563314'
  _elasticsearch_indexed_on: '2010-02-12 09:00:00'
  _wpcom_is_markdown: '1'
---
Its pretty straight forward to generate a simple web page, there are several software tools available that will generate the mark up code for you. However, even using these tools, it is easy to misuse tags and not conform to the <a href="http://www.w3.org/TR/xhtml11/">XHTML standards</a>. If you were unfortunate enough to follow the previous link, you may well be thinking,

<blockquote>   What does it all mean, my web page looks fine so does compliance really matter?   </blockquote>

This is a valid point, however compliance is widely regarded to be important for a number of reasons. These are pretty <a href="http://www.leemunroe.com/how-important-is-valid-html-web-standards/">well documented</a> around the web so I   ll just highlight a couple of the reasons that I think are most important.

Firstly a compliant web page should be assured of displaying fairly consistently across all major browsers, both now and in the future. Unfortunately some browsers are more compliant than others, internet explorer still demands some extra attention but it is much improved since the days of IE6. The newer browsers which have come onto the market, such as Firefox and Chrome, generally display compliant pages very well. Secondly I think that compliance is important as it makes the content more accessible to machine readers. For example the readers used by search engines will be able to read and index the page content easier, which will help increase traffic to the site. This theory has actually been tested and there is some evidence to back up the claims that <a href="http://www.hobo-web.co.uk/seo-blog/index.php/official-google-prefers-valid-html-css/">Google prefers valid HTML and CSS</a>.

<h4>Keep it clean</h4>

The first step to compliance is to separate the HTML mark-up from the presentation using a CSS (Cascading Style Sheet) document. This helps ensure that the web page is short and the content is easy to find. Apparently search engines are busy guys and they just don   t have the time to be reading all that content on your site. So they only read up to a certain number of characters, if your important content is pushed too far down the document then there is a rick that it will never get indexed.

I   ve gone for a fairly straight forward layout on the recipe stash site, with a fixed width page of 960 pixels to target resolution widths of 1024 and above. The header, footer and main content are all in a single column. This layout can be created in CSS without too much trouble. I   ve tried to use the HTML tags as they were intended originally, for example the menus are tagged as an unordered list of elements, then styled to display horizontally.

There is an unfortunate limitation with HTML, it does not support curved corners. So I   ve used to <a href="http://www.html.it/articoli/niftycube/index.html">Nifty Corners</a> to create the curved effect on the top navigation menu and latest recipes section:

<a href="http://lh3.ggpht.com/_5hQeqbPSrks/S3SKU8XP19I/AAAAAAAAAEg/LyqqweIM2DE/s1600-h/recipestash-nav%5B4%5D.jpg"><img title="recipestash-nav" src="http://lh5.ggpht.com/_5hQeqbPSrks/S3SKVdVr1qI/AAAAAAAAAEk/xCnYOpYSNAI/recipestash-nav_thumb%5B2%5D.jpg?imgmax=800" border="0" alt="recipestash-nav" width="492" height="56" /></a>

Nifty Corners is a JavaScript based tool which adds HTML and CSS to the page when it is loaded to create the effect of curved corners. The other option I considered to create the curved corners was to cut out images of the corners themselves and position them in the corners of the HTML elements. However I decided to use Nifty corners for a number of reasons:

<ul>
    <li>No images are required</li>
    <li>The generated HTML mark-up is compliant</li>
    <li>The implementation is fast     just one line of JavaScript to call!</li>
    <li>It degrades gracefully (If JavaScript is not available then it does nothing)</li>
</ul>

<h4>Exceptions to the rule</h4>

I mentioned earlier that internet explorer interprets style sheets differently to other browsers. This is so pronounced for version six that a completely different style sheet would be needed to create the same visual layout. I   ve decided that this would take up too much of my time and have instead used the <a href="http://forabeautifulweb.com/blog/about/universal_internet_explorer_6_css/">universal ie6 style sheet</a>, along with an upgrade message for anyone that is using IE6.

I   m currently running on Windows Vista and it is pretty tricky to test for all versions of internet explorer. There are two main methods that can be used to test, either install a virtual machine running Windows XP with IE6 installed natively or use the <a href="http://www.my-debugbar.com/wiki/IETester/HomePage">IETester</a> program. I   ve gone for the IETester program, mainly because it is quicker to install and doesn   t take up a huge chunk of my hard drive. I   ve tried it out and it displays the basically styled page I would expect to see in IE6 mode:

<a href="http://lh5.ggpht.com/_5hQeqbPSrks/S3SKV6LNNrI/AAAAAAAAAEo/-kTCMp0wAF0/s1600-h/IETester2%5B3%5D.jpg"><img style="border:0 none;" title="IETester2" src="http://lh4.ggpht.com/_5hQeqbPSrks/S3SKWZVC3DI/AAAAAAAAAEs/JO8fB75k_mA/IETester2_thumb%5B1%5D.jpg?imgmax=800" border="0" alt="IETester2" width="481" height="471" /></a>

<span style="color:#ff0000;"><span style="color:#000000;">The deficiencies of Internet Explorer versions seven and eight are less pronounced and I only needed to modify a couple of elements to give the same appearance to the web page:</span></span>

<ul>
    <li>Added fixed widths to the main div elements so margins are applied</li>
    <li>Extra div element wrapper on the so the margin is applied</li>
    <li>Add fixed widths to the navigation menu elements to fix nifty corners in ie7</li>
</ul>

<h4>Validating the page</h4>

It would be difficult to judge when the mark up and styling of a web page is actually valid, fortunately there are many tools freely available on the web to help. During development I generally use the <a href="http://users.skynet.be/mgueury/mozilla/">HTML validator</a> Firefox extension. This provides a little icon in the bottom right corner of the web browser that indicates whether the current page is valid or not. Clicking the icon brings up a list of validation errors for the page. I like this tool as it is quick to browse through a site and check the validation on each page.

I   ve also included links to the <a href="http://www.w3.org/">World Wide Web Consortiums</a> XHTML and CSS <a href="http://validator.w3.org/">validation services</a> in the page footer. This will submit the current page to their online validation tools and display a report with suggestions for any problems that are found.

Well that   s just about all I   ve got on compliance, however if you   re after a more    hard line    message then you can <a href="http://www.entertonement.com/clips/fhzjhvmbgv--You-have-20-seconds-to-complyRobocop-Jon-Davison-Ed-209-">check this out</a>!
