---
layout: post
title: Behaviour Driven Design encourages shared understanding
categories:
- Software Design
tags:
- BDD
- Unit Testing
status: publish
type: post
published: true
date: 2011-10-23 11:21:38
meta:
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  _wpas_skip_fb: '1'
  _wpas_skip_ms: '1'
  _elasticsearch_indexed_on: '2011-10-23 11:21:38'
  _wpcom_is_markdown: '1'
---
Most recent projects I have worked on have used unit tests  to define the requirements of our code and ensure they are met. Typically the tests are written by the developer while they are coding the solution. However this means that the tests are based entirely on the developers understanding of the requirements, which may not always be the same as the stakeholders understanding.

To combat this issue we are introducing <a href="http://dannorth.net/introducing-bdd/">behaviour driven development</a>, to agree on a set of requirements for each feature with the stakeholders before starting development. The requirements are captured using the Gherkin syntax (Given, When, Then) based on real world examples. These examples can then be wired up to the system directly as automated tests using <a href="http://specflow.org/">Specflow</a>.

<h2>OK, show me the example already</h2>

Here is a fictionalised scenario which demonstrates the value that BDD adds to a project. An abbreviated version of the initial scenarios drafted by the developer are as follows:

<pre style="font-size:small;font-family:Consolas, 'Courier New', Courier, Monospace;width:100%;overflow:auto;border:1px dashed #999999;background-color:#eeeeee;"><span style="color:#0000ff;font-weight:bold;">Feature</span>: Process 3D secure authentication response
        As a logged in user
        I would like to place a 3D secure authenticated order
        so that I have assurance of the websites security

<span style="color:#0000ff;font-weight:bold;">Scenario</span>: Handle a 3D secure response of AUTHORISED
    <span style="color:#0000ff;"><strong>Given </strong></span>a 3D secure response of AUTHORISED
        <span style="color:#0000ff;font-weight:bold;">When</span>  i view the order confirmation page
        <span style="color:#0000ff;font-weight:bold;">Then</span>  the order status will be Complete

<span style="color:#0000ff;font-weight:bold;">Scenario</span>: Handle a 3D secure response of NOT AUTHORISED
    <span style="color:#0000ff;"><strong>Given </strong></span>a 3D secure response of NOT AUTHORISED
        <span style="color:#0000ff;font-weight:bold;">When</span>  i view the order confirmation page
        <span style="color:#0000ff;font-weight:bold;">Then</span>  the order status will be Cancelled

<span style="color:#0000ff;font-weight:bold;">Scenario</span>: Handle a 3D response of NOT PROCESSED
    <span style="color:#0000ff;"><strong>Given </strong></span>a 3D secure response of NOT PROCESSED
        <span style="color:#0000ff;font-weight:bold;">When</span>  i view the order confirmation page
        <span style="color:#0000ff;font-weight:bold;">Then</span>  the order status will be Cancelled</pre>

<h2>Evolving the scenario</h2>

The assumptions shown in these scenarios are perfectly reasonable, an order will only be completed if the 3D secure security check returns an authorised response. However it should really be a decision for the stakeholder's business whether the system should behave in this way. There are several valid scenarios where all 3D responses should actually result in a completed order, for example:

<pre style="font-size:small;font-family:Consolas, 'Courier New', Courier, Monospace;width:100%;overflow:auto;border:1px dashed #999999;background-color:#eeeeee;"><span style="color:#0000ff;font-weight:bold;">Scenario</span>: Handle a 3D secure response of NOT AUTHORISED
<span style="color:#0000ff;"><strong>Given</strong></span> a 3D secure response of NOT AUTHORISED
<span style="color:#0000ff;"><strong>And</strong></span> a total order value order value under the following amount:   25
<span style="color:#0000ff;font-weight:bold;">When</span>  i view the order confirmation page
<span style="color:#0000ff;font-weight:bold;">Then</span>  the order status will be Complete</pre>

The stakeholders calculated that the risk of charge-backs was so low on orders under   25 that they could disregard the 3D secure response completely and allow 'not authorised' transactions to be completed.

Here we can see a clear benefit in the stakeholder sharing their understanding of the system requirements with the implementation team. Rules which are beneficial to their business can be incorporated into the system, resulting in a more effective solution from the outset.

If you are interested in reading more about using BDD for web applications I can recommend <a href="http://blog.stevensanderson.com/2010/03/03/behavior-driven-development-bdd-with-specflow-and-aspnet-mvc/">Steven Sanderson's excellent blog post</a>.
