---
layout: post
title: How to handle the MVC HttpAntiForgeryException
categories:
- Tips and Tricks
tags:
- C#
- MVC
status: publish
type: post
published: true
date: 2011-02-10 21:24:16
comments: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-02-10 21:24:16'
  _wpcom_is_markdown: '1'
---
When developing forms for websites it is good practice to guard against <a href="http://en.wikipedia.org/wiki/Cross-site_request_forgery">cross site request forgery</a> attacks. In MVC it is straightforward to do this using the <em>AntiForgeryToken()</em> helper method and the  <em>ValidateAntiForgeryToken </em>attribute (<a href="http://blog.stevensanderson.com/2008/09/01/prevent-cross-site-request-forgery-csrf-using-aspnet-mvcs-antiforgerytoken-helper/">as explained by Steve Sanderson</a>). However a side effect of this is that an  <em>HttpAntiForgeryException </em>like this will be thrown whenever a forgery attack is prevented:

<blockquote>System.Web.Mvc.HttpAntiForgeryException: A required anti-forgery token was not supplied or was invalid.</blockquote>

This is not generally a problem unless the site has been setup to notify you of any exceptions by email. Then you may find that your inbox quickly fills up with spam error reports and I think there is already quite enough spam in the world!
The fix for this is to catch the exception before it makes it out of the application and into your inbox. The exception can be caught by adding the <em>HandleError </em>attribute to the controller class declaration as shown below:

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:150px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>[HandleError(ExceptionType=typeof(HttpAntiForgeryException), View="Unauthorised")]
public class MyController : Controller
{
  ...
   public ActionResult Unauthorised()
  {
     return View("Unauthorised");
  }
}
</code></pre>

Once the attribute has been added the <em>Unauthorised </em>view should be shown if a forgery attempt is intercepted. However in my case a different exception of the type shown below  was thrown instead:

<blockquote>The model item passed into the dictionary is of type 'System.Web.Mvc.HandleErrorInfo' but this dictionary requires a model item of type 'Core.Web.ViewModels.ISomeViewModel'.</blockquote>

<span style="font-family:Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif;line-height:19px;white-space:normal;font-size:13px;">This exception is thrown because (as described  <a href="http://aspnet.codeplex.com/workitem/1795">in this work item</a> on the MVC issue tracker) the default view model has been replaced by a <em>HandleErrorInfo </em>object and our master page is trying to access properties in the normal view model. So the straightforward fix for this is to remove the dependancy on the master page from the <em>Unauthorised </em>view. </span>
