---
layout: post
title: Microsoft jump on the Open Source band wagon
categories:
- Progress
tags:
- MVC
- Open source
status: publish
type: post
published: true
date: 2012-04-03 11:35:04
comments: true
meta:
  tagazine-media: a:7:{s:7:"primary";s:78:"http://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Tux.png/220px-Tux.png";s:6:"images";a:1:{s:78:"http://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Tux.png/220px-Tux.png";a:6:{s:8:"file_url";s:78:"http://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Tux.png/220px-Tux.png";s:5:"width";s:3:"220";s:6:"height";s:3:"261";s:4:"type";s:5:"image";s:4:"area";s:5:"57420";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"1";s:6:"author";s:8:"12339140";s:7:"blog_id";s:8:"11998060";s:9:"mod_stamp";s:19:"2012-04-03
    11:35:04";}
  _wpas_done_linkedin: '1'
  publicize_results: a:1:{s:7:"twitter";a:1:{i:33610861;a:2:{s:7:"user_id";s:12:"michael_hook";s:7:"post_id";s:18:"187141168319045633";}}}
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2012-04-03 11:35:04'
  _wpcom_is_markdown: '1'
---
<img class="alignright" title="Tux, the Linux Mascot" src="http://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Tux.png/220px-Tux.png" alt="" width="220" height="261" />
A landmark decision was made last week by Microsoft's ASP.NET web development team. They announced that three key technologies used to develop web applications, <a href="http://weblogs.asp.net/scottgu/archive/2012/03/27/asp-net-mvc-web-api-razor-and-open-source.aspx">ASP.NET MVC, Web API and Razor would be made fully Open Source</a><sup>1</sup>. This is a big step for a company built on the sales of Closed Source software. I'm going to have a look at what I think this decision means for the consumers, who use software built with these technologies, and the stakeholders who commission the software.

<h2>Open Sesame</h2>

For those those not involved directly in software development, the moniker 'Open Source' may conjure up images of magicians pulling rabbits out of a hat<sup>2</sup>. However the reality could be viewed as the opposite of magic as Open source demystifies the software development process. In the traditional 'Closed Source' licencing model, software is made by a company behind closed doors and the code is encrypted before release to the public. The traditional thinking is that this protects the company by preventing third parties from stealing their intellectual property. This is a subject of <a href="http://en.wikipedia.org/wiki/Software_patent_debate">some debate</a>  , which I won't go into here. However it is clear that many businesses are currently successful without relying on restrictive software patents.

In an Open Source licencing model, the full unencrypted source code will be published at the same time as the software itself (indeed often before-hand in the case of preview and beta releases). Also, as in the case of this announcement, public contributions may be accepted to modify the original source code. Naturally the acceptance process is completely at the discretion of software company, so unhelpful or damaging contributions will not be accepted.

A good example of the Open Source model working commercially is the Android operating system released by Google. A massive range of mobile devices run Android and it is not only the key competitor to Apple's iOS (iPhones &amp; iPad operating system but has helped ensure that most devices using Microsoft's Windows Mobile operating system are sitting on the scrap heap. I for one am glad that there is healthy competition to the world of Apple and its clone army of i-device accolytes.

<h2>Software that Just Works</h2>

From a consumer or stakeholder viewpoint the natural desire is to want software that 'just works' and to reach that goal we just need to fix all the bugs in the software. Unfortunately the belief that all software bugs can be fixed is generally  infeasible  given time and budget restraints, due to the nature of programming the two go hand in hand. In each piece of code there are many paths that can be travelled, much like a maze in a country mansion garden. If you ask 10 different people to walk the maze the chances are each one will set off in a different direction. Most of them will hit a dead end before finding their way through and a nasty bug could be hiding at each of these dead ends!

However Open Source software helps minimize the number bugs by accepting contributions to fix any that are found by the development community. In this way the software benefits from the knowledge of a much wider group of people than could be acheived if it was developed and maintained soley by a single company. Most software developers are driven by that itch to 'make stuff work' and are only too happy to contribute to a project which helps make this happen. The end result is a product that is more reliable for the consumers and stakeholders and has the appearance of 'just working'.

<h2>Security concerns</h2>

Typically the biggest obstruction to making software Open Source is the perceived risk to security. If anybody can view exactly how the software is operating then it is much easier for hackers to cause the software to break, exploit a vulnerability to propogate a virus or carry out other nefarious acts. The <a href="http://haacked.com/archive/2012/03/29/asp-net-mvc-now-accepting-pull-requests.aspx">indications by Phil Haack</a>, a former member of the MVC team, are that this decision by Mircosoft is no exception. So a great deal of credit should go to those involved at Microsoft for making change happen in their organization.

Regarding the perceived risks to security, Open Source can actually reduce the risk for similar reasons to the improvement in relibility. Any security vulnerabilities will quickly be picked up by the community and fixed. The Open Source culture of continual improvement in the software makes for a much more secure product.

Take the example of web browsers, a couple of years ago Microsoft's Internet Explorer browser had massive market share. However over time so many security vulnerabilities were exploited in the browser that the <a href="http://www.theinquirer.net/inquirer/news/1037530/us-government-warns-internet-explorer">US government themselves warned against using the software</a>!   Along came Mozilla's Firefox browser to save the day, with much improved security. Naturally Firefox has always been developed under an Open Source licence as explained in the <a href="http://www.mozilla.org/about/manifesto.html">Mozilla manifesto</a>.

<h2>Future developments</h2>

The likelyhood is that this announcement will not trigger a deluge of community updates and fixes to the frameworks in question. I anticipate that a minority of change submissions will make it back into the frameworks themselves. However it does demonstrate an important change in mindset from Microsoft and may well signal the beginnings of a new approach for the organisation. It is clear that in the fast moving world of software and digital media you need to adapt to survive and it is encouraging that even massive organisations such as Microsoft are able to do this.

<h5>Footnote</h5>

<sup>1</sup>While the MVC framework has been open source since its original release, this announcement widens the license model to the Web API and Razor frameworks and also marks the first time that Microsoft will accept public contributions to the open source frameworks. If you are unfamiliar with these technologies then further information is available on the Microsoft blah blah (LINK).

<sup>2</sup>Or maybe thats just my fertile imagination!
