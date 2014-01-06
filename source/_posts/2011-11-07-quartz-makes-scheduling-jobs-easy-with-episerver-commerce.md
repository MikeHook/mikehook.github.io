---
layout: post
title: Quartz makes scheduling jobs easy with EPiServer Commerce
categories:
- Software Design
tags:
- EPiServer
- Open source
- Services
status: publish
type: post
published: true
meta:
  _wpas_done_linkedin: '1'
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  _wpas_skip_ms: '1'
  _elasticsearch_indexed_on: '2011-11-07 19:30:00'
  _wpcom_is_markdown: '1'
---
<a href="http://quartznet.sourceforge.net/index.html"><img class="alignright" title="Quartz rock" src="http://upload.wikimedia.org/wikipedia/commons/thumb/1/14/Quartz,_Tibet.jpg/240px-Quartz,_Tibet.jpg" alt="Quartz rock" width="168" height="169" /></a>A common requirement for e-commerce applications is the ability to schedule jobs or tasks. I am currently working on a project which uses EPiServer Commerce and I needed to schedule an order export job.

I was pleasantly surprised to find that EPiServer Commerce is supplied bundled with the <a href="http://quartznet.sourceforge.net/index.html">Quartz.NET</a>  enterprise code library to facilitate these job scheduling requirements.

<h3>Gentlemen, choose your weapons</h3>

I had three main options available for implementing the export job, a separate windows service, an EPiServer CMS scheduled job or a Quartz job. I decided that the Quartz service was better than the option of a separate windows service because the framework was already there and available so would not increase the complexity of deployments. I also felt it was a better option than a EPiServer CMS scheduled job because Quartz runs  independently  to the website app pool so should not put the web application under greater load while it is running and potentially degrade the performance of the end user website.

<h3>A 1000 foot example</h3>

I'm not going to go into a massively detailed explanation of how to implement the jobs as there is a already a good example available in the <a href="http://sdk.episerver.com/commerce/1.1.1/Content/Developers%20Guide/Architecture/LongRunProcessScheduling.htm">EPiServer Commerce dev guide</a>  along with a detailed <a href="http://quartznet.sourceforge.net/tutorial/index.html">tutorial on the Quartz .NET site</a>. However a quick overview demonstrates how intuitive it is to implement.

The first thing you need to do is create your job class which will run when Quartz triggers the execution. The class must inherit from the IJob interface which just has a single Execute method to implement. Here is a sample skeleton class:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">    public class CommerceOrderExportJob : IStatefulJob
    {
        private readonly ILog _log;

        public CommerceOrderExportJob()
        {
            _log = LogManager.GetLogger(base.GetType());
            //Any other init code
        }

        public void Execute(JobExecutionContext context)
        {
            //Your export methods
        }
    }</pre>

You then need to extend the config file to configure Quartz to call your job. I think all the parameters are pretty self explanatory:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">&lt;job&gt;
    &lt;job-detail&gt;
      &lt;name&gt;OrderExportJob&lt;/name&gt;
      &lt;group&gt;all&lt;/group&gt;
      &lt;description&gt;This job exports orders from commerce&lt;/description&gt;
      &lt;job-type&gt;Your.Assembly.CommerceOrderExportJob, Your.Assembly&lt;/job-type&gt;
      &lt;volatile&gt;false&lt;/volatile&gt;
      &lt;durable&gt;true&lt;/durable&gt;
      &lt;recover&gt;false&lt;/recover&gt;
    &lt;/job-detail&gt;
    &lt;trigger&gt;
      &lt;simple&gt;
        &lt;name&gt;OrderExportJobTrigger&lt;/name&gt;
        &lt;group&gt;eCommerceFramework&lt;/group&gt;
        &lt;description&gt;Fires Order Export Job&lt;/description&gt;
        &lt;misfire-instruction&gt;SmartPolicy&lt;/misfire-instruction&gt;
        &lt;volatile&gt;false&lt;/volatile&gt;
        &lt;job-name&gt;OrderExportJob&lt;/job-name&gt;
        &lt;job-group&gt;eCommerceFramework&lt;/job-group&gt;
        &lt;repeat-count&gt;RepeatIndefinitely&lt;/repeat-count&gt;
        &lt;repeat-interval&gt;300000&lt;/repeat-interval&gt;
      &lt;/simple&gt;
    &lt;/trigger&gt;
  &lt;/job&gt;</pre>

Hit run on the service and check the event log for any problems. Make sure to include some useful event/error logging in the job otherwise it will just be a mysterious black box! There are a couple of snags you may run into when running the job against EPiServer Commerce:

<div>
<ul>
    <li>ensure that your compiled assembly is actually available in the Quartz service directory, this is different from the main commerce site bin folder so it is not enough just to include it in the commerce app references</li>
    <li>Make sure the data context has been initialised and the config file in the Quartz service directory is pointing to your database</li>
</ul>
</div>

<h3>Mining for more gems</h3>

Ultimately only time will tell if Quartz was the best choice but I feel it has the best chance of high reliability and performance in the long term. I hope that, where appropriate, EPiServer can continue to expand the incorporation of proven open source libraries with their product offerings.
