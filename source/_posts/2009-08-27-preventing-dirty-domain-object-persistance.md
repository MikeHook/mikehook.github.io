---
layout: post
title: Preventing Dirty Domain Object Persistance
categories:
- Software Design
tags:
- DDD
status: publish
type: post
published: true
date: 2009-08-27 19:24:00
comments: true
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '6565641647390612946'
  _elasticsearch_indexed_on: '2009-08-27 19:24:00'
  _wpcom_is_markdown: '1'
---
The add recipes feature has currently been implemented in a way that does not allow for dirty domain objects to be persisted (saved to the database). The recipe object constructor must be passed all the essential object properties to prevent a partial object from being persisted, as shown below:<br />
<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;font-size:12px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>        public Recipe(string title, IList recipeIngredients, string cookingMethod) : this()
        {
            //TODO - Add the recipe creator (ie. user) to the domain signature
            Check.Require(title != null &amp;&amp; title.Trim() != string.Empty, "title must be provided.");
            Check.Require(recipeIngredients != null, "recipeIngredients must be provided.");
            Check.Require(cookingMethod != null &amp;&amp; cookingMethod.Trim() != string.Empty, "method must be provided.");
            Title = title;
            Ingredients = recipeIngredients;
            CookingMethod = cookingMethod;        
        }
</code></pre>

<br />
However this means that the state of the recipe object must be maintained across the 3 separate add recipe views. I've had to use both hidden fields on the views and the TempDataDictionary to maintain this state. It has become clear to me that there may be a better way to implement this if I use a RecipeDraft domain object in addition to the Recipe object. However making this change will take a while so I need to weigh up whether I think it is worth while.<br />
<br />
<b><span style="font-size:small;">Advantages</span></b><br />

<ul><li>Users can add draft recipes and return later to complete them</li>
<li>More readable controller methods (RecipeIngredientDtos is a nasty object name!) <br />
</li>
<li>No need to maintain temporary state across views (using hidden fields and the TempDataDictionary)</li>
<li>Simplifed View Models</li>
</ul>

<b>Disadvantages</b><br />

<ul><li>Time to refactor the code and unit tests</li>
<li>More records in the database (potentially higher cost)<br />
</li>
<li>More I/O operations on the database (slower website page loads)</li>
</ul>

On balance I think the advantages are outweigh the disadvantages, so I will be refactoring the code to use a RecipeDraft object. This will be the constructor of the RecipeDraft object:<br />
<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;font-size:12px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>        public RecipeDraft(string title) : this()
        {
            //TODO - Add the recipe creator (ie. user) to the domain signature
            Check.Require(title != null &amp;&amp; title.Trim() != string.Empty, "title must be provided.");
            Title = title;      
        }
</code></pre>
