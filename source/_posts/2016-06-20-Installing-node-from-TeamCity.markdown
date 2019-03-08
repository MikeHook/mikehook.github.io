---
layout: post
title: Installing node modules in a TeamCity build
comments: true
categories:
- Tips and Tricks
date: 2016-06-20
published: true
---

[<img src="/images/posts/nodejs-logo.jpg" class="alignleft" title="node js" />](https://nodejs.org)

Keeping up with the inexorable rise of front end frameworks is a big challenge in modern web development is. My pathway is probably fairly common, starting out with [jQuery](https://jquery.com/) and the humble selector, progressing onto [Knockout](http://knockoutjs.com/) and embracing the joy of binding then finally onto the full on framework smorgasbord of [Angular JS](https://angularjs.org/). Now there is a new project on the horizon and Angular 2 has reached a good level of maturity, so "Once more unto the breach, dear friends" it is time to embrace the new all over again!

Our project stack is a fairly standard .net picture, an ASP.NET MVC application on the front end talking to a REST API server side based on Web Api. Angular 2 sits firmly in the front end application and can be installed through node. I won't go into details of that setup here as Deborah Kurata has already covered that off in her excellent [Angular 2 Getting started with Visual Studio post](http://blogs.msmvps.com/deborahk/angular-2-getting-started-with-a-visual-studio-2015-asp-net-4-x-project/), however the point where I hit trouble was getting the application to build on our TeamCity build server.

##Its Javascript Jim, but not as we know it

A big benefit of Angular 2 is its built in support for coding in TypeScript, a strongly typed  superset of JavaScript that can be transpiled into JavaScript before it is executed. While TypeScript has been around for quite a while now (compared to Angular 2 anyway!) and is baked into Visual Studio I still needed to get the newest version to enable visual studio to build the scripts. This was simply a case of downloading the latest version of [TypeScript for Visual Studio 2015](https://www.microsoft.com/en-us/download/details.aspx?id=48593) and installing.

Once the installation step had been repeated on our build server the compiler also ran through TeamCity. So far so good, however we didn't have the node_modules folder checked into source control (as they are external dependencies containing a crazy number of files!) so the Angular 2 source libraries were not present. Naturally this was a sticking point in getting our javascript code to compile or indeed run as Angular 2 didn't exist in the application on the build server! To fix this we needed to download all the dependencies in the node_modules folder during the build and save them into the expected location in the build servers workspace.

##Hey node, give me the dependencies

TeamCity has a great plugin model which allows it capabilities to be easily extended and a quick google revealed that a [TeamCity Node plugin](https://github.com/jonnyzzz/TeamCity.Node) already existed. After installing the plugin, along with Node on the build server, I was able to add a build step to execute the following command:

`npm install`

When this command is executed from the directory of our MVC application it interrogates the **package.json** file and installs all the dependencies listed in the json file. Well, that's what it **should** do, in my case all I got was a long pause followed by a network timeout, doh!

I was prepared for this as we are working behind a corporate firewall and I'd already spent quite some time finding out how to get a connection from my development machine. The solution, using the Cntlm Authentication proxy, is handily documented on StackOverflow in this [NPM behind NTLM proxy article](https://stackoverflow.com/questions/18569054/npm-behind-ntlm-proxy/18570201#18570201). However after following all the steps on the build server the network connections were still failing. I tried running the npm install locally on the build server and it worked fine so it seemed that the npm proxy settings were not being used when the program ran through TeamCity. After quite a bit of head scratching I resorted to reading the [npm manual](https://docs.npmjs.com/files/npmrc) and there were some clues as to why the config might be ignored. When I looked in my C:\Users\\{MyUsername}\ folder I could see a .npmrc file with the proxy settings, however there was no such file in the C:\Users\\{TeamCityAgentUser}\ folder. So I copied my .npmrc file into here then bingo, it all worked through TeamCity!

I'm sure this won't be the last challenge Angular 2 throws at our Dev Ops efforts but we are in a good place now with the build running inside TeamCity.