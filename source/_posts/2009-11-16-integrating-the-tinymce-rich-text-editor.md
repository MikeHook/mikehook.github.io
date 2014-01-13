---
layout: post
title: Integrating the TinyMCE Rich Text Editor
categories:
- Hacking
tags:
- CSRF
- TinyMCE
- XSS
status: publish
type: post
published: true
date: 2009-11-16 20:45:00
comments: true
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '5079943566689815431'
  _elasticsearch_indexed_on: '2009-11-16 20:45:00'
  _wpcom_is_markdown: '1'
---
I   ve been working on a page to add method descriptions for the recipes. The most simple solution for this would be a large text area allowing the user to add multiple lines of text for the recipe method. However I think this would be too limiting for the users and also offer poor formatting when the completed recipes are viewed. A better solution is to use a rich text editor which allows users to entered formatted text and generates HTML to use on the recipe views.

The editor had to allow the users to enter formatted text including paragraphs, text highlighting, numbered and bulleted lists. It was also important that the editor could be limited to prevent users entering content which would break the styling of the site. For example no image tags should be entered in the method as there is already a separate image property for each recipe. Fortunately I   ve had experience using the <a href="http://tinymce.moxiecode.com/">TinyMCE rich text editor</a> from working on <a href="http://n2cms.com/">N2 CMS</a> based websites at work and this editor meets all of the requirements.

Integrating TinyMCE into the site was pretty straightforward, there is detailed documentation on the <a href="http://wiki.moxiecode.com/">TinyMCE wiki</a>, including installation instructions. Adding the editor to the site is simply a case of downloading the assets, including them in the VS project and including a javascript call to load the editor in the page header. By default the call transforms all text areas into TinyMCE editors. I also found the simple mode provided the limited feature set I was looking for. There were just a couple of issues to resolve before the editor could be used.

<h4>Disable Input Validation</h4>

The editor generates HTML which is posted back to the server when the page is submitted. However by default MVC throws an exception as the postback contains HTML tags:

<blockquote><strong>Exception Details: </strong>System.Web.HttpRequestValidationException: A potentially dangerous Request.Form value was detected from the client (CookingMethod="&lt;p&gt;A Test method&lt;/p&gt;
&lt;u...").</blockquote>

To prevent this exception from being thrown the page validation must be disabled. This is done by adding the <em>[ValidateInput(false)]</em> attribute to the controllers action method which is called when the page is submitted.

<h4>Protect Against Unwanted HTML / XSS</h4>

After making this change the editor content posts back to the server and can be saved to the repository. However disabling the input validation leaves the site open to cross site scripting (XSS) attacks so some extra precautions need to be put in place.
TinyMCE already checks the content which is posted back, however the wiki points out that the editor can be easily disabled by any user by disabling javascript in the browser. They recommend that some server side checks are put in place to prevent any unwanted tags and scripts from being persisted to the repository.
I   ve added some code to check the recipe method string and strip all unwanted tags. It uses a regular expression to allow a subset of tags to be saved. Any other tags are replaced by an empty string. I found a good <a href="http://stackoverflow.com/questions/307013/how-do-i-filter-all-html-tags-except-a-certain-whitelist">SantizeHTML method</a> posted on stack overflow, so no need to reinvent the wheel here!

<h4>Protect Against CSRF</h4>

While I was in the area of security I thought I may as well increase the protection of the form to cross site request forgery (CSRF) attacks. Steve Sanderson has already written a full <a href="http://blog.codeville.net/2008/09/01/prevent-cross-site-request-forgery-csrf-using-aspnet-mvcs-antiforgerytoken-helper/">blog post</a> on this subject (among others I   m sure) so I won   t go into detail on this. Suffice to say I thought it was worth adding as it only takes 5 minutes and provides an extra level of protection which may be needed later.
