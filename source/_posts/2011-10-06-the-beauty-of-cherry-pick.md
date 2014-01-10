---
layout: post
title: The beauty of cherry pick
categories:
- Tips and Tricks
tags:
- git
- Testing
status: publish
type: post
published: true
date: 2011-10-06 17:00:59
meta:
  _wpas_done_twitter: '1'
  _wpas_skip_yup: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_ms: '1'
  _wpas_skip_327859: '1'
  _elasticsearch_indexed_on: '2011-10-06 17:00:59'
  _wpcom_is_markdown: '1'
---
I'm loving the <a href="http://gitready.com/intermediate/2009/03/04/pick-out-individual-commits.html">cherry pick command</a> in git right now, this is the scenario that it really helps me out for.

I've fixed a bug on the live branch, however before it can be published to the live website it needs to be tested. So I can first publish it to our UAT (user acceptance testing) site so the client can test the changes. However the UAT site already has a bunch of other unreleased features on it. If I merge my bug fix branch into the UAT branch it will overwrite some of the other feature changes.

The solution here is to 'cherry pick' just my changes from my bug fix branch and commit those to the UAT branch. In this way git doesn't merge all the other differences between the live and UAT branch.
