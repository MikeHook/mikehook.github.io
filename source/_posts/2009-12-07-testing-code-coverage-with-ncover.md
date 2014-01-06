---
layout: post
title: Testing code coverage with NCover
categories:
- Testing
tags:
- NCover
- NUnit
- TDD
- Unit Testing
status: publish
type: post
published: true
meta:
  blogger_author: Mike Hook
  blogger_blog: bakingwebsites.blogspot.com
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '4368657861627991904'
  _elasticsearch_indexed_on: '2009-12-07 21:47:00'
  _wpcom_is_markdown: '1'
---
I   ve been developing the recipe book website mostly using TDD (Test Driven Design) and I thought it would be interesting to measure the code coverage of my unit tests. After researching what tools were available it looked like <a href="http://www.ncover.com/">NCover</a> was the best fit for the sites technology stack. The commercial version has some good features, such as measures for cyclomatic complexity, however it is a bit out of my budget (of approximately   0!). Fortunately a <a href="http://www.ncover.com/download/community">community edition of NCover</a> is also available for free and it stills works just fine for analysing current .NET assemblies.

After installing NCover a console application is available for running code coverage analysis. I got this up and running but the command line interface isn   t exactly the most user friendly and the output is just a single summary report. I obviously wasn   t alone in these conclusions because back in 2006 Grant Drake developed the <a href="http://www.kiwidude.com/blog/2006/01/ncoverexplorer-debut.html">NCoverExplorer tool</a> to provide a GUI front end to NCover. This tool also has a community edition available (version 1.4) that works with the community edition of NCover.

<h4>Starters - Setting up NCover Explorer</h4>

The tool is fairly straight forward to setup, the NCover website includes instructions on how to setup the professional edition to <a href="http://docs.ncover.com/how-to/running-ncover-with-your-unit-testing-framework/nunit/">profile an NUnit test project</a>, and the settings are equivalent in the community edition. I also chose to specify which assemblies should be included in the coverage report as otherwise all the Sharp Architecture assemblies are included and the results are skewed.

After running the tool you are provided with an interactive view of the assemblies with the uncovered lines displayed in red. You can see how easy it is to identify the offending lines in the screen grab below:

<a style="margin-bottom:1em;margin-right:1em;" href="http://4.bp.blogspot.com/_5hQeqbPSrks/Sx15F3sFlqI/AAAAAAAAAA8/yrLagOXdmxc/s1600/NCoverExplorer2.jpg"><img src="http://lh3.ggpht.com/_5hQeqbPSrks/Sx1745WamzI/AAAAAAAAAB4/PAdEtdGUeBE/s800/NCoverExplorer2.jpg" border="0" alt="" /></a>

<h4>The Main Course     Improving code coverage</h4>

The results of my first report are shown below:

<a href="http://bakingwebsites.files.wordpress.com/2009/12/ncoverreport1.jpg"><img class="alignnone size-full wp-image-31" title="NCover Report" src="http://bakingwebsites.files.wordpress.com/2009/12/ncoverreport1.jpg" alt="" width="745" height="698" /></a>

The prime candidates for improvement were:

<ul>
    <li>The RecipeBook.Data assembly. A couple of repository classes were not tested.</li>
    <li>The RecipeBook.Web.Controllers assembly. Some of the code paths in the controller methods were not covered.</li>
    <li>The RecipeBook.ApplicationServices assembly. Not tested at all, oops!</li>
</ul>

By default NCover sets the acceptable coverage level at 95%, however I felt that this was overly strict. I would rather spend the time writing fewer tests that I ensure are of a high quality than writing tests just to cover every single line of code. So I reduced the threshold to 80% and added tests to cover the essential missing features. Below if my much more healthy looking report.

<h4>For Desert     A report without the red sauce</h4>

OK maybe I   m stretching the food analogy a little too far with my headings now! By red sauce I mean those nasty red bars that were blazed across nearly every line of the report. Below you can see the report looks much more healthy after adding some unit tests and trimming some unused code:

<a href="http://bakingwebsites.files.wordpress.com/2009/12/ncoverreport2.jpg"><img class="alignnone size-full wp-image-32" title="NCover Report - 80% coverage" src="http://bakingwebsites.files.wordpress.com/2009/12/ncoverreport2.jpg" alt="" width="790" height="790" /></a>

I will endeavour to keep the coverage level at a minimum of 80% from now, and hopefully add some more tests to bump off those remaining pesky red bars!

As usual, any comments on this post are welcome. :)
