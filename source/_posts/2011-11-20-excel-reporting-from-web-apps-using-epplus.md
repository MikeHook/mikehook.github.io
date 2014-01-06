---
layout: post
title: Excel reporting from web apps using EPPlus
categories:
- Software Design
tags:
- Excel
- Open source
- Reporting
status: publish
type: post
published: true
meta:
  _wpas_done_linkedin: '1'
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  _wpas_skip_ms: '1'
  tagazine-media: a:7:{s:7:"primary";s:0:"";s:6:"images";a:0:{}s:6:"videos";a:0:{}s:11:"image_count";s:1:"0";s:6:"author";s:8:"12339140";s:7:"blog_id";s:8:"11998060";s:9:"mod_stamp";s:19:"2011-11-20
    15:26:02";}
  _elasticsearch_indexed_on: '2011-11-20 15:20:16'
  _wpcom_is_markdown: '1'
---
I have recently been working on a new feature request for the ability to generate excel reports dynamically from a web application. This kind of feature can add a lot of value to a system as it enables businesses to analyse crucial trends in data on demand.

However it can be challenging to generate reports in an Excel spreadsheet format from a web application. Firstly you need to have the program itself installed on the web server, which can be difficult if you are using third party hosting. Also  coding the solution itself is difficult as you either need intimate knowledge of the Excel API or, for versions of office after 2007, in depth knowledge of the OpenXML file format. But fear not there is a third option, open source to the rescue! Some clever chaps have developed the <a href="http://epplus.codeplex.com/">EPPlus library</a> which abstracts away all the nasty low level OpenXML calls and provides a nice object oriented API for dealing with directly the spreadsheet.

<h2>Modelling the report data</h2>

I won't go into too much detail on the features of the library as you can read all about it on the <a href="http://epplus.codeplex.com/wikipage?title=FAQ">EPPlus FAQ page</a> and download the samples. However one of the methods that I found really useful was the <em>LoadFromArrays</em>  method. This enables a table of data to be output to the worksheet from a list of object arrays. So you can construct a model of the data you want to display then feed it straight to the worksheet through the list. It may sound quite complicated but here is a simple example that can be easily expanded with more columns / rows:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">string firstName = "Joe";
string lastName = "Bloggs";
var dateOfBirth = new DateTime(2000, 1, 1);
var testData = new List&lt;object[]&gt;()
                   {
                       new object[] {"First name", firstName},
                       new object[] {"Last name", lastName},
                       new object[] {"Date of birth", dateOfBirth}
                   };

using (var excelPackage = new ExcelPackage())
{
    ExcelWorksheet excelWorksheet = excelPackage.Workbook.Worksheets.Add("Test worksheet");
    //Loads the summary data into the sheet, starting from cell A1. Prints the column names on row 1
    excelWorksheet.Cells["A1"].LoadFromArrays(testData);
}</pre>

<h2>Don't get duped</h2>

I generally found that the EPPlus library was reliable, however I did hit a snag during the development. The file would get generated OK but when I tried to open it in Excel a nasty message was shown:

<blockquote>Excel found unreadable content in 'Test export.xlsx'. Do you want to recover the contents of this workbook?</blockquote>

The file did open OK if you chose to recover, however this obviously isn't ideal and not acceptable for a client release. The tricky part was that no error was being thrown so I had to use a bit of guess work to track down the issue. Initially there wasn't much to go on, however after running some more tests on sample data I noticed that the export always worked fine if the file only included a single worksheet. However sometimes multiple worksheets would cause the 'unreadable content' error. From there I examined each of the properties that were being set on the worksheets and narrowed the problem down to be the worksheet name itself.

It turns out that excel requires each worksheet to have a unique name. This is backed up in excel itself if you try and manually name to worksheets the same you get a reasonably friendly error message saying:

<blockquote>Cannot rename a sheet to the same name as another sheet</blockquote>

After ensuring my worksheets were uniquely named the reports always opened up without a problem. As an aside I just tried to replicate this bug through EPPlus and this time it returned a much more helpful error message, I wish it had told me this when I was originally working on the feature!

<blockquote>Add worksheet Error: attempting to create worksheet with duplicate name</blockquote>

<h2>EPPlus for the win</h2>

In conclusion I would recommend EPPlus to anybody who needs to generate an excel report from C# code and doesn't want to interface with the murky depths of the Excel API. It is a great open source library that does just what you need in the way you would expect it to work.
