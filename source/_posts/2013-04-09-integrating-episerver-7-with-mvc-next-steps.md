---
layout: post
title: Integrating EPiServer 7 with MVC - Next Steps
categories:
- Hacking
tags:
- EPiServer
- MVC
status: publish
type: post
published: true
date: 2013-04-09 16:40:42
meta:
  publicize_twitter_user: michael_hook
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:33610861;b:1;}}
  _wpas_done_327858: '1'
  _wpas_done_327859: '1'
  _wpas_skip_327859: '1'
  _wpas_skip_327858: '1'
  _elasticsearch_indexed_on: '2013-04-09 16:40:42'
  _wpcom_is_markdown: '1'
---
My previous blog post, <a title="Integrating EPiServer 7 with an existing MVC  site" href="http://bakingwebsites.co.uk/2013/03/14/integrating-episerver-7-with-an-existing-mvc-site/">Integrating EPiServer 7 with an existing MVC site</a>, outlined the initial steps to get EPiServer 7 running. However there are a number of additional steps you may need to take before your site is fully integrated.

<h3>Convert any Controllers not in Areas</h3>

If your site only contains controllers within Areas then you should not need to make any changes. However controllers in the root of the site will not work by default (at least for me). I found it was best to deal with these controllers on a case per case basis as the best course of action depended on the function of the controller. I implemented one of the following changes for each:

<ul>
    <li><span style="line-height:13px;">Move the controller into an area. This option makes sense if the views for the controller do not have any CMS editable content</span></li>
    <li>Convert the controller to be CMS managed. Makes sense if the views need to be CMS editable (inherit from  PageController&lt;T&gt; where T is a BasePage)</li>
    <li>Convert the controller to an HttpHandler. In one case the controller did not have any views or models, it was really just a different entrypoint to the site which ran a bit of logic and redirected. In this case there was no point in retaining the controller as an HttpHanlder could do the same job more efficiently</li>
</ul>

<h3>Disable strict language routing</h3>

I encountered a further issue after patching my EPiServer.Core and EPiServer.Framework nuget packages to the latest versions. None of the controller actions in the home controller worked any more except for the Index. This was fairly easy to remedy by disabling the strict language routing, via the episerver/sites/site/strictLanguageRouting config setting.

<h3>Deployment Issues</h3>

While its all well and good having the site running on your machine, eventually it will need to be deployed somewhere. We handle configuration changes with file transforms and I was pleased to note that the config changes required for episerver deployments are now minimised. Here is a summary list of all the settings we changed per environment:

<ul>
    <li><span style="line-height:13px;">connectionStrings
</span></li>
    <li>episerver/sites/site/siteSettings/siteUrl</li>
    <li>episerver.framework/siteHostMapping/siteHosts</li>
    <li>episerver.framework/appData/basePath</li>
    <li>episerver.framework/licensing/licenseFilePath</li>
    <li>episerver.search/namedIndexingServices/services/baseUri</li>
</ul>

Note, you may need to transform further settings for a load balanced site.

<h5>EPiServer UI not deployed</h5>

Previously with EPiServer v6 all that was needed to run the CMS interface itself was to install the application on the web server. However the EPiServer 7 UI architecture also has a number of dependencies in the {VPP}\modules folder. So ensure that these files are also present in your deployed environment, otherwise you will probably encounter the following error:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">DirectoryNotFoundException: Could not find a part of the path '{Your site path} \Website\ClientResources\ClientResources\packages.config'</pre>

<h5>Search service configuration</h5>

We have the search service configured on a local host name as the indexing service does not like to communicate over a public host name. However this caused a further issue with the service reporting that multiple hosts names could not be run for the service. Fortunately this could be remedied easily by setting the system.serviceModel/serviceHostingEnvironment/multipleSiteBindingsEnabled  setting to true.

Those were all the steps we needed to take to deploy the site successfully. I'd be interested to hear your experiences if you found any other settings that need to be changed. Feel free to reply in the comments below or contact me directly on twitter <a href="https://twitter.com/michael_hook">@michael_hook</a>.
