---
layout: post
title: Branching the Project
categories:
- Hacking
tags:
- Testing
- TortoiseSVN
- Unfuddle
status: publish
type: post
published: true
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '8740943601947286469'
  _elasticsearch_indexed_on: '2009-08-23 21:02:00'
  _wpcom_is_markdown: '1'
---
I have used an SVN repository for the project, hosted on <a href="http://www.unfuddle.com/">unfuddle</a>, enabling versioned source control on all files in the project. I can use the trusty <a href="http://tortoisesvn.net/">TortoiseSVN</a> Windows application to send and receive files to the remote repository directly from Windows Explorer. After each work session I commit the changes to source control. Apart from the obvious advantages of maintaining backups, if at any stage I realise that I have made a mistake, then the changes can be rolled back to the previous commit.<br />
The source control recently came in useful when I needed to upgrade the version of Sharp Architecture used on the project. I won   t go into detail about the reasons for me upgrading, suffice to say most of the technology stack in Sharp Architecture had benefitted from version updates and I was limiting myself by using a pre version 1.0 release.<br />
These are the steps I took to upgrade the Sharp Architecture version of the project:<br />

<ol><li>     <br />
Use TortoiseSVN to Branch the code in source control from <i><span style="color:green;">/trunk</span></i> to <i><span style="color:green;">/branches/recipebookV1</span></i><br />
</li>
<li>     <br />
Rename the working folder (ie. the local folder in Windows Explorer) to recipebookV1 and switch it to the branch.<br />
</li>
<li>     <br />
Update the Sharp Architecture project to fetch v1.0 from source control and copy the new project template into the Visual Studio templates folder.<br />
</li>
<li>     <br />
Generate a new Visual Studio project, named recipebook, from the v1.0  Sharp Architecture templates.<br />
</li>
<li>     <br />
Use <a href="http://www.scootersoftware.com/">Beyond Compare</a> to copy all the files that I have manually added / changed in the recipebookV1 project to the new recipebook project<br />
</li>
<li>     <br />
Add the files to the recipebook project in Visual Studio, cross fingers, compile and run!<br />
</li>
<li>     <br />
Use the TortoiseSVN repo browser to delete all files from the Trunk    yes, I really deleted them all! <br />
</li>
<li>     <br />
Import the new recipebook folder back into the trunk <br />
</li>
</ol>

After completing the upgrade I realised that there may be an easier way to upgrade, without the scary and somewhat bad practice of deleting all the old files from the trunk, only to upload most of them again. I will list them below so I can try them out later:<br />

<ol><li>     <br />
Use TortoiseSVN to Branch the code in source control from <i><span style="color:green;">/trunk</span></i> to <i><span style="color:green;">/branches/recipebookV1</span></i><br />
</li>
<li>     <br />
Rename the working folder (ie. the local folder in Windows Explorer) to recipebookN. <i>This enables a new visual studio project of the same name to be created, with namespaces which are the same as the working project.</i><br />
</li>
<li>     <br />
Update the Sharp Architecture project to fetch v1.0 from source control and copy the new project template into the Visual Studio templates folder.<br />
</li>
<li>     <br />
Generate a new Visual Studio project named recipebook from the V1.0 Sharp Architecture templates.<br />
</li>
<li>     <br />
Use <a href="http://www.scootersoftware.com/">Beyond Compare</a> to copy all updated Sharp Architecture files from the new recipebook project to the working recipebookn folder<br />
</li>
<li>     <br />
Add any extra files to the working recipebook project in Visual Studio, cross fingers, compile and run!<br />
</li>
<li>     <br />
Commit the changes to the trunk, remove the new recipebook project and rename the existing recipebook project folder to recipebook.  <br />
</li>
</ol>
