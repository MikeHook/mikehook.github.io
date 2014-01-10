---
layout: post
title: How to delete old log4net database records
categories:
- Tips and Tricks
tags:
- C#
- Logging
status: publish
type: post
published: true
date: 2011-01-04 19:43:12
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-01-04 19:43:12'
  _wpcom_is_markdown: '1'
---
We have log4net setup to record events from a web application in a database.  However it had been running for a couple of weeks and the database table was getting rather large (144,000 records at last count).  This was causing performance issues when accessing the database table and also taking up quite a bit of disk space.  Obviously the database needed some kind of maintenance script to keep the size under control.

I investigated the log4net support for maintaining the size of the database table  and there is none built in as such. However the configuration is fairly  verbose and it allows you to define the SQL script itself which is run to log the events. So with a fairly straightforward amendment to this script I was able to keep the database size in check:

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:280px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>
&lt;appender name="EventLogAdoNetAppender" type="log4net.Appender.AdoNetAppender"&gt;          
  &lt;bufferSize value="0" /&gt;          
  &lt;connectionType value="System.Data.SqlClient.SqlConnection, System.Data, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" /&gt;
  &lt;connectionString value="{your conn string}" /&gt;
  &lt;commandText&gt;
    &lt;![CDATA[INSERT INTO EventLog ([Date],[Thread],[Level],[Logger],[Message],[Exception])
             VALUES (@log_date, @thread, @log_level, @logger, @message, @exception);
             DELETE FROM EventLog WHERE Date &lt; DATEADD(Day, -7, GETDATE())]]&gt;
  &lt;/commandText&gt;
</code><span style="font-family:monospace;">  &lt;parameter&gt;
    &lt;parameterName value="@log_date" /&gt;
    &lt;dbType value="DateTime" /&gt;             
    &lt;layout type="log4net.Layout.RawTimeStampLayout" /&gt;          
  &lt;/parameter&gt;
  ...
&lt;/appender&gt;</span></pre>

The key changes are to the commandText element. The default command just runs an insert query, this has been updated above to also run a delete query. This will remove any records in the EventLog table which are over 7 days old.
It can easily be changed to a different record age by amending the second parameter of the DATEADD method.
It was also necessary to wrap the command in a CDATA element because the delete query contains an reserved XML character (&lt;).

I'm aware that this is not the most efficient way to implement this, an alternative would be to add a scheduled task to the SQL Server Agent, however we do not have access to this on the database server.
Also I prefer how the maintenance is built into the log4net configuration here so it will now work immediately on any database server without the need for separate configuration steps.
