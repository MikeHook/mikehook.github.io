---
layout: post
title: Integrating EPiServer 7 with an existing MVC site
categories:
- Hacking
tags:
- EPiServer
- MVC
status: publish
type: post
published: true
date: 2013-03-14 15:54:57
comments: true
meta:
  publicize_reach: a:2:{s:7:"twitter";a:1:{i:327859;i:160;}s:2:"wp";a:1:{i:0;i:1;}}
  publicize_twitter_user: michael_hook
  _wpas_done_327859: '1'
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:33610861;b:1;}}
  _wpas_done_327858: '1'
  _wpas_skip_327859: '1'
  _wpas_skip_327858: '1'
  _elasticsearch_indexed_on: '2013-03-14 15:54:57'
  _wpcom_is_markdown: '1'
---
<img class="alignright size-full wp-image-408" alt="custom_integration" src="http://imageshack.com/a/img560/4853/3rqu.jpg" width="300" />This post assumes you have EPiServer 7 installed on your machine. If you don't have it then it is available from <a href="http://world.episerver.com/Download/Categories/Products/EPiServer-CMS/">EPiServer world</a>  (once you have created an account).

There are 2 main options for creating a new EPiServer 7 MVC stylee site:

<ul>
    <li><span style="font-size:13px;">Create a new Visual studio project (after installing 'EPiServer 7 Visual Studio Integration', available from </span><a style="font-size:13px;" href="http://world.episerver.com/Download/Categories/Products/EPiServer-CMS/">EPiServer world</a><span style="font-size:13px;">)</span></li>
    <li><span style="font-size:13px;line-height:19px;">Use the </span><a style="font-size:13px;line-height:19px;" href="http://world.episerver.com/Articles/Items/ASPNET-MVC-Templates-for-EPiServer-7-CMS/">EPiServer 7 MVC templates package</a></li>
</ul>

For integrating with an existing site I'd recommend using option 1, as there are far less files to integrate. However it is well worth looking at the templates package as well as this demonstrates how models and views can be implemented with EPiServer 7.

After creating the new Visual Studio project I remembered that EPiServer have a NuGet feed, and they do have the version 7 assemblies on NuGet. However the version numbers are slightly different to those used in the visual studio project, so make sure you specify the version numbers to NuGet when you install the packages (they are listed below). Here are the steps I followed in detail:

<ol>
    <li><span style="line-height:13px;">Created a new Visual studio project using the EPiServer Web Site (MVC) template
</span></li>
    <li>Followed the EPiServer tutorial (<strong>link</strong>) to add a home page and set it to the 'start page'</li>
    <li>Upgraded my existing site to MVC 4 by installing the Microsoft ASP.NET MVC 4 nuget package (obviously not required if your existing site is already on MVC version 4.</li>
    <li>Copy across the connectionStrings.config, episerver.config,  EPiServerFramework.config, EPiServerLog.Config and FileSummary.config files</li>
    <li>Update the  siteUrl in  episserver.config.</li>
    <li>Copy the AppData, IndexingService, modules and modules bin folders.</li>
    <li>Merge the web.config files - ensure the  episerver.search baseUri is updated to point at your site address.</li>
    <li>Installed EPiServer 7 from Nuget feeds using these commands to get the correct versions:
<ul>
    <li>Install-Package EPiServer.Framework -Version 7.0.859.1</li>
    <li>Install-Package EPiServer.CMS.Core -Version 7.0.586.1</li>
</ul>
</li>
    <li>Removed assembly references to Razor and System.WebPages.Razor (as was clashing with EPiServer versions)</li>
    <li>Update Controllers to inherit from  PageController&lt;T&gt; where T is a PageData class.</li>
    <li>Update Global.asax to inherit from  EPiServer.Global</li>
</ol>

<h2><strong>IoC containers clashing!</strong></h2>

OK, this is the tricky bit it gets quite involved but please stay with me!

When I tried to run the site it now returned a 'No parameterless construct or defined for this object.' message. Which in plainer  English  means the application can't create the Controller classes. This is because the site I am integrating with currently uses the Castle Windsor IoC container, however EPiServer uses the StructureMap IoC container internally. We  definitely  don't want two IoC containers and I can't see any evidence of the EPiServer container being swappable. So the only option here is to convert the current Windsor implementation to a StructureMap implementation. More information on Structure map can be found on the excellent <a href="http://docs.structuremap.net/">StructureMap documentation site</a>, however I think it is worth including an overview in this post as, whilst this isn't always going to be necessary for your site, I expect it will be a common problem.

<h4>Add StructureMap version 2.6.1.0</h4>

<span style="font-size:13px;">This is available from NuGet (as this is the version used in the EPiServer 7 MVC templates package).
The command to install is:  </span><span style="font-size:13px;">Install-Package StructureMap  -Version  2.6.1.0</span>

<h4>Register your services with StructureMap</h4>

<ul>
    <li><span style="font-size:13px;">Copy the  StructureMapDependencyResolver and DependencyResolverInitialization  classes from the EPiServer MVC templates project.</span></li>
    <li>Convert service registration calls to register services with StructureMap instead of Windsor. This is carried out in the  ConfigureContainer method of the  DependencyResolverInitialization class.</li>
</ul>

For example here is a Windsor interface registration and its equivalent StructureMap registration:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">//Windsor registration - Here container is an implementation of IWindsorContainer
var myConfig = WebConfigurationManager.GetSection("myConfig") as MyConfig;
container.Register(Component.For(typeof(IMyConfig)).Instance(myConfig));</pre>

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">//StructureMap registration - Here container is of type ConfigurationExpression
var myConfig = WebConfigurationManager.GetSection("myConfig") as MyConfig;
container.For&lt;IMyConfig&gt;().Use(myConfig);</pre>

An example of a class registration with a singleton lifestyle:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">//Windsor registration
container.Register(Component.For(typeof(Cache)).Instance(HttpContext.Current.Cache).LifestyleSingleton());</pre>

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">//StructureMap registration
container.For(typeof(Cache)).Singleton().Use(HttpContext.Current.Cache);</pre>

<ul>
    <li><span style="font-size:13px;">Convert  </span><em style="font-size:13px;">IWindsorInstaller</em><span style="font-size:13px;">  implementations  to  </span><em style="font-size:13px;">Registry</em><span style="font-size:13px;">  subclasses.</span></li>
</ul>

<h4>Debugging your StructureMap registrations</h4>

You can debug structure map by adding this line:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">_container.AssertConfigurationIsValid();</pre>

Initially EPiServer was reporting many registration failures but you can delay the call until the InitComplete event is called in the DependencyResolverInitialization class. Then most of the EPiServer  registrations have completed and it is easier to see if there are any issues with your own services.

<h4>Manually retrieving a service</h4>

Generally my services are injected into constructors automatically, however I had a couple of calls to retrieve services manually from the IoC container. For example, to retrieve an implementation of an interface called IMyService:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">_var myService = DependencyResolver.Current.GetService&lt;IMyService&gt;();</pre>

<h4>Last steps</h4>

Hopefully your site will be running now, there is some additional configuration work you may want to do for EPiServer 7 VPP folders. But if you've made it this far you will probably need a break!

<h5>Updated on 9th April 2013</h5>

Updated the list of config files which must be copied to include EPiServerLog.config and FileSummary.config
