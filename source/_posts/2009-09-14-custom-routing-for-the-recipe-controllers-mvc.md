---
layout: post
title: Custom Routing for the Recipe Controllers (MVC)
categories:
- Hacking
tags:
- MVC
- Routing
status: publish
type: post
published: true
date: 2009-09-14 07:45:00
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '4594923828190712698'
  _elasticsearch_indexed_on: '2009-09-14 07:45:00'
  _wpcom_is_markdown: '1'
---
I've been working on re-factoring the add recipe feature to include a draft recipe domain object. However the recipe controller has been steadily expanding, along with the number of unit tests to support it. This was causing me issues as it was getting difficult to scan through the files to find the methods I'm interested in. So I've introduced some custom routing and split up the recipe controller.<br />
<br />
<b>Original recipe controller signature </b><br />
This was the recipe controller signature before introducing the customer routing (for brevity in this post I've extracted the controller signature to an interface, the interface is not actually used in the project):<br />
<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;font-size:12px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>    interface IRecipesController
    {
        ActionResult Index(string title);

        ActionResult EditIngredients(string recipeTitle);
        ActionResult EditIngredients(RecipeIngredientsModel ingredientsModel);

        ActionResult EditMethod(string recipeTitle);
        ActionResult AddRecipeIngredient(RecipeIngredientsModel ingredientsModel);
        ActionResult SearchIngredients(RecipeIngredientsModel ingredientsModel);
        ActionResult EditMethod(RecipeMethodModel methodModel);

        ActionResult EditSummary(string recipeTitle);
        ActionResult EditSummary(RecipeSummaryModel summaryModel);   
    }
</code></pre>

<br />
So that's 9 public methods, along with some private methods, taking the total up to around 15. I thought that was too many for the class to be easily manageable.<br />
<br />
<b>Custom Routing Implementation</b><br />
<br />
In Sharp Architecture projects the custom routing instructions need to be added in the RouteRegistrar.cs file, located in the Web.Controllers assembly. Handily, the Sharp architecture example project had an example similar to what I was trying to achieve, so I just needed to tweak the area and route URL's to make my customer routing:<br />
<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;font-size:12px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>    routes.CreateArea("Recipes", "RecipeBook.Web.Controllers.Recipes",
        routes.MapRoute(null, "Recipes/{controller}/{action}", new { action = "Index" }),
        routes.MapRoute(null, "Recipes/{controller}/{action}/{id}")
    );
</code></pre>

<br />
<b>Coding Tweaks<br />
</b><br />
<br />
An interesting result of this routing change was the tweaks that needed to be made to the code:<br />

<ul><li>The action links in Views must now use the Html.ActionLinkForAreas() method instead of Html.ActionLink<i></i>()</li>
<li>RedirectToAction calls in controllers must include the controller name </li>
<li>Tests for Controller Actions, which redirect to another controller, should make use of the ToController(<i>controllerName</i>) MvcContrib TestHelper extension method.</li>
</ul>

<b>Revised controller signatures</b><br />
<br />
After implementing the custom routing the controllers and tests could be broken up into 4 separate files with the following method signatures:<br />
<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;font-size:12px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>    interface IRecipesController    
    {
        ActionResult Index(string title);
    }
    interface ISummaryController    
    {
        ActionResult Edit(string recipeTitle);              
        ActionResult Edit(RecipeSummaryModel summaryModel); //Post        
    }
    interface IMethodController    
    {
        ActionResult Edit(string recipeTitle);
        ActionResult Edit(RecipeMethodModel methodModel);   //Post
    }
    interface IRecipeIngredientsController
    {
        ActionResult Edit(string recipeTitle);
        ActionResult AddRecipeIngredient(RecipeIngredientsModel ingredientsModel);  //Post
        ActionResult Edit(RecipeIngredientsModel ingredientsModel);                 //Post    
        ActionResult SearchIngredients(RecipeIngredientsModel ingredientsModel);    //Post
    }
</code></pre>
