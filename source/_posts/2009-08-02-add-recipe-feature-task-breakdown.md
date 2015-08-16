---
layout: post
title: Add recipe feature - task breakdown
categories:
- Hacking
tags:
- sharp architecture
- TDD
status: publish
type: post
published: true
date: 2009-08-02 16:54:00
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '650370260021610783'
  _elasticsearch_indexed_on: '2009-08-02 16:54:00'
  _wpcom_is_markdown: '1'
---
<span>The add recipe feature is the first feature that will added to the site. This task can first be broken down into 4 subtasks:
</span>

<ol><li>Add a recipe summary</li><li>List / add ingredients
</li><li>Add recipe ingredients</li><li>Add a recipe cooking method</li></ol>

<span>However, the sharp architecture framework is designed for Test Driven Development. These are the TDD steps listed in the </span><span>sharp architecture</span><span> reference guide (in the docs folder of the SVN project):</span>

<blockquote><ol><li>Write your test as if the target objects and API already exists.</li><li>Compile the solution and see it break.</li><li>Write just enough code to get it to compile.</li><li>Run the test and see if fail.</li><li>Write just enough code to get it to pass.</li><li>Run the test and see it pass!</li><li>Refactor if necessary!</li></ol></blockquote>

Taking another look at the feature subtasks  from a TDD perspective, the breakdown for each domain object looks more like this:
<span>
<span style="font-weight:bold;">Ingredient</span>
</span>

<ol><li>Add tests for the ingredient domain object and controller
</li><li>Implement the ingredients domain object and controller to make the tests pass</li><li>Add Ingredients views for List and Create  </li></ol>

<span style="font-weight:bold;">
Recipe</span>

<ol><li>Add tests for the recipe domain object and controller</li><li>Implement the recipe domain object and controller to make the tests pass</li><li>Add Recipe views for CreateSummary, CreateIngredients, CreateCookingMethod</li></ol>

<span>
Currently I just need to implement the Create view for the Ingredient object, then I can move onto recipes.</span>