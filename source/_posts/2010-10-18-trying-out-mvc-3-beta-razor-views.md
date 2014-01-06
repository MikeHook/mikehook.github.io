---
layout: post
title: Trying out MVC 3 Beta Razor Views
categories: []
tags:
- Uncategorized
status: draft
type: post
published: false
meta:
  _oembed_dc051a8500189e2bacf9560f7960e0a8: ! '{{unknown}}'
---
Ref: http://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx
<h2>Layout pages</h2>
<h3>Adding extra sections</h3>
The first problem I encountered was how to add an extra section that could be filled out from the view, in MVC 2 parlance an extra ContentPlaceHolder.

This can be achieved by adding the following markup to the layout file:
<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>@RenderSection("header", required:false)
</code></pre>
If the required parameter is set to true an exception will be thrown if the view does not include the section. You can output html to this section from the view using the following syntax:
<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>@section header {
  html goes here
}
</code></pre>
How to comment out lines??

http://weblogs.asp.net/gunnarpeipman/archive/2010/10/18/asp-net-mvc-3-beta-razor-views-support-comments-now.aspx

&nbsp;
