---
layout: post
title: "Automated Azure deployments"
date: 2014-07-02 10:29:24 +0100
comments: true
categories: 
- Progress
published: true
---

<img src="http://imagizer.imageshack.us/v2/320x240q90/850/r38o.jpg" class="alignleft" alttext="Wall-e the robot" />

Azure web site hosting has a great built in feature for deployment automation. All you need to do is point Azure at the location of your website in your source control platform and it will automatically monitor for updates, pull them into Azure and deploy them, Boom! Well that is the theory anyway, turns out in my case I needed to do some tweaking to get the automation to work.

## Tinkering under the hood

The first step to set up the automated deployment is involves going to your Azure website dashboard and selecting 'Setup up deployment from source control'. There are a bunch of options for what source control services are supported and how they can be setup, this is all pretty well documented in the [Azure publishing from source control article]("https://azure.microsoft.com/en-us/documentation/articles/web-sites-publish-source-control/") so I won't rehash all of that here. Suffice to say I pointed the website at my github repo and sorted out all the authentication, then Azure pulled through a fresh version to deploy.

Unfortunately when I checked the shiny new 'Deployments' tab I found that the deployment had failed, after looking in the error log the reason was clear enough:

> "Error: The site directory path should be the same as repository root or a sub-directory of it."

My website was not in the root folder of the repository, it is in a 'website' folder as you can see in the [repo here]("https://github.com/MikeHook/MSTC"). so I needed to tell whatever magic was running these deployments to check in that folder instead for the code changes. After a bit of googling I found out that the deployments are driven by an application called kudu which has some documenation on its [github wiki]("https://github.com/projectkudu/kudu/wiki"). Turns out that it is pretty straight forward to modify the folder, as explained on the [customising deployments wiki page]("https://github.com/projectkudu/kudu/wiki/Customizing-deployments") I just had to add a .deployment file to the repository root with these contents:

    [config]
    project = website

Simples, the deployment worked fine after adding that file... well it did when I just tried it but previously it didn't seem to work. Either I made some stupid syntax error previously or kudu got fixed since last time I tried! 

The actual website is running a different configuration based on a custom deployment script, while this is a little OTT to just change the folder path going the extra mile paid dividends later on when I needed to make some other customisations during the deployment. It was pretty straightforward to set up, thanks to the azure-cli tool which generates a deployment script for you based on a set of parameters. Instructions on how to do this are on the [kudu wiki deployment hooks page]("https://github.com/projectkudu/kudu/wiki/Deployment-hooks"). In my case I just needed to run the following command from my repository root to generate a working .deployment and deploy.cmd file. 

    azure site deploymentscript --aspWebSite -r website
    
Once checked in those files are used by kudu to control the automated deployment process. Check back in Azure and the deployment should now be showing as successful, awesome!


