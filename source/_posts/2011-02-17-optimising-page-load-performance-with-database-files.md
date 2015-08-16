---
layout: post
title: Optimising page load performance with database files
categories:
- Software Design
tags:
- NHibernate
status: publish
type: post
published: true
date: 2011-02-17 19:19:43
comments: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-02-17 19:19:43'
  _wpcom_is_markdown: '1'
---
I recently ran into an issue with a newly released web application. The test version of the application was performing fine, however the live version kept reporting  time out  errors.
The page that was generating the time outs included a list of links to files stored in a database. After some investigation it became clear that the issue was being caused by the application loading all of these files into memory during the page load (yikes!).

It had worked fine on the test site because the bandwidth between the database and the website was a lot higher and also only a couple of small files had been stored. However the live site was hitting the HTTP request timeout (90 seconds by default) before all of the files had finished loading into memory. There was not actually any need for these files loaded at all during page load, so I set about re-factoring the domain structure to reflect this.

<h3>Hurry up and be more lazy!</h3>

The application uses NHibernate to map the domain objects to an SQL database, this is an example similar to the object that was causing the performance issue:

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:100px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>public class StoredFile
{
    public virtual string Name { get; set; }
    public virtual string Extension { get; set; }
    public virtual bool Saved { get; set; }
    public virtual byte[] Contents { get; set; }
}
</code></pre>

You can see here that the FileContents property is an array of bytes, which will be loaded into memory when the StoredFile object is initialised. However the only properties we need to access during the page load are the FileName, FileExtension and FileSaved properties.

If the FileContents property is moved to a separate object and then NHibernates <a href="http://martinfowler.com/eaaCatalog/lazyLoad.html">lazy loading</a> feature can be leveraged to only load that object if it is accessed. The revised domain structure looks like this:

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:170px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>public class StoredFile
{
    public virtual string Name { get; set; }
    public virtual string Extension { get; set; }
    public virtual bool Saved { get; set; }
    public virtual StoredFileContents FileContents { get; set; }
}

public class StoredFileContents
{
    public virtual byte[] Contents { get; set; }
}
</code></pre>

<h3>NHibernate Mapping Issues</h3>

You may have noticed that the StoredFileContents class above does not include a reference to the parent StoredFile object. Originally I had included this reference and attempted to map it following <a href="http://ayende.com/Blog/archive/2009/04/19/nhibernate-mapping-ltone-to-onegt.aspx">Ayende's one-to-one mapping blog post</a>. However I hit problems when trying to save, as each record needed to know the primary key of the other record. This was only workable if one of the foreign key tables was set to nullable. In this case, as I don't  foresee  there being a need to access a StoredFile from a StoredFileContents instance, I decided it was better to keep the foreign key as non nullable and only map the relationship in one direction.

In case you're wondering what effect the change had on the application, the  page load time was reduced to a couple of seconds (and even less on a decent connection), sorted!