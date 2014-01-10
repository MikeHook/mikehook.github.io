---
layout: post
title: Troubleshooting a web service SoapException
categories:
- Tips and Tricks
tags:
- C#
- Web Services
status: publish
type: post
published: true
date: 2011-01-06 20:13:22
meta:
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  _wpas_skip_fb: '1'
  _wpas_skip_ms: '1'
  _elasticsearch_indexed_on: '2011-01-06 20:13:22'
  _wpcom_is_markdown: '1'
---
I recently encountered this exception consuming a web service from an application:

<blockquote>System.Web.Services.Protocols.SoapException:  Server did not recognize the value of HTTP Header SOAPAction: http://service.foo.com/baa.asmx</blockquote>

The code ran fine locally and only caused an exception running on the web server. Initially I thought it could be to do with the web server running a newer version of the .NET framework. But this proved not to be the case.

Eventually I got the issue tracked down and it was a simple typo in the settings on the live environment.
The system has the web service addresses stored in a database and the application assigns these dynamically at runtime (via the <em>Url </em>property). We have it  set-up  this way to enable the test application to send to a different end point from the live application. However this does rely on the same service being available on the end point and in this case it wasn't.

So we were changing the URL to be:

<blockquote>http://service.foo.com/baa.asmx</blockquote>

But it needed to point to here instead:

<blockquote>http://service.foo.com/baas.asmx</blockquote>

Note the <em>s </em>on the end of the service method name, one little character causing one pesky SOAP exception!
