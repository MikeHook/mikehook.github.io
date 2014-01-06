---
layout: post
title: Optimising data access queries (Part 1)
categories:
- Software Design
tags:
- Linq
- NHibernate
- sharp architecture
status: publish
type: post
published: true
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '1502107523739367246'
  _elasticsearch_indexed_on: '2009-10-11 15:11:00'
  _wpcom_is_markdown: '1'
---
In this post I will be looking at the best way to re-factor a data access query which is used on the add ingredients page. Initially the query was written quickly with no consideration given to the time it would take to execute. It is not currently a problem as the database is very small but in future the page could be very slow to load.<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:40px;line-height:14px;overflow:auto;width:100.55%;padding:5px;"><code>    ingredientsModel.MatchingIngredients = IngredientRepository.GetAll()
        .Where(i =&gt; i.Name.ToLower().Contains(ingredientsModel.SearchIngredient.ToLower())).ToList();
</code></pre>

This is a poorly optimised query as the generic <a href="http://wiki.sharparchitecture.net/ClassLibraries.ashx#SharpArchData_2">Sharp Architecture repository</a> <i>GetAll()</i> method call retrieves the entire data set from the database. It is then filtered in memory to get the actual matching ingredients. For larger datasets, it would be much quicker if the data is filtered before it is retrieved from the database. The query could be rewritten to use the <i>FindAll(IDictionary propertyValuePairs)</i> method like this:<br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:32px;line-height:14px;overflow:auto;width:100.72%;padding:5px;"><code>    ingredientsModel.MatchingIngredients = IngredientRepository
        .FindAll(new Dictionary { { "Name", ingredientsModel.SearchIngredient.ToLower()} });</code></pre>

This does solve the optimisation issue as the resulting database SQL query uses a <i>Where</i> clause to pre-filter the data. However, the query is limited to retrieving exact matches to the search string. It is unlikely that users will input the exact ingredient they are searching for, so it would be better if the query retrieved all ingredients that contain the search term. <br />

<h4>
Unit Testing the Custom Repository</h4>

In order to do this I will use a custom repository and add my own query method. Much credit is due to the <a href="http://wiki.sharparchitecture.net/SettingUpNorthwind.ashx">Sharp Architecture example projects</a>, which provide a good basis for the techniques I   ve used here. First, I'll write a unit test for the new repository method. The test class is shown below and each section is explained after the class: <br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:471px;line-height:14px;overflow:auto;width:100.88%;padding:5px;"><code>    [TestFixture]
    public class IngredientRepositoryTests : RepositoryTestsBase
    {
        protected override void LoadTestData()
        {
            CreatePersistedIngredient(1, "Chicken thigh", FoodGroup.Meat);
            CreatePersistedIngredient(2, "Chicken breast", FoodGroup.Meat);
            CreatePersistedIngredient(3, "Red Pepper", FoodGroup.Vegetable);
            CreatePersistedIngredient(4, "Jalapeno Chilli", FoodGroup.Vegetable);
        }

        private IIngredientRepository ingredientRepository = new IngredientRepository();

        private Ingredient CreatePersistedIngredient(int ingredientId, string ingredientName, FoodGroup foodGroup)
        {
            Ingredient ingredient = new Ingredient(ingredientName, foodGroup);
            ingredientRepository.SaveOrUpdate(ingredient);
            FlushSessionAndEvict(ingredient);
            return ingredient;
        }

        [Test]
        public void CanFindIngredientsByName()
        {
            List&lt;Ingredient&gt; ingredients = ingredientRepository.FindByName("chicken").ToList();
            ingredients.ShouldNotBeNull();
            ingredients.Count.ShouldEqual(2);
        }
    }
</code></pre>

<ul>
<li>The unit test inherits from the RepositoryTestsBase (namespace SharpArch.Testing.NUnit.NHibernate) which enables test data to be easily loaded into the in-memory database by overriding the the LoadTestData method. </li>
<li>The ingredientRepository property is the custom repository data access class </li>
<li>The CreatePersistedIngredient method is used in the LoadTestData method to save a single ingredient entity to the in-memory database. Note the call to FlushSessionAndEvict is used to persist the saved entity to the database </li>
<li>The CanFindIngredientsByName method is our test method which runs the repository FindByName method&nbsp; </li>
<li>The test method asserts that the ingredients list is populated with 2 Ingredient entities </li>
</ul>

<h4>
Implementing the Custom Repository </h4>

Now we are ready to implement the custom repository for the accessing ingredient data. Again, the class is shown below with explanations following it: <br />

<pre style="background-color:#eeeeee;border:1px dashed rgb(153,153,153);color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:110px;line-height:14px;overflow:auto;width:101.54%;padding:5px;"><code>    public class IngredientRepository : Repository&lt;Ingredient&gt;, IIngredientRepository
    {        
        public IEnumerable&lt;Ingredient&gt; FindByName(string ingredientName)
        {
            return Session.Linq&lt;Ingredient&gt;().Where(a =&gt; a.Name.Contains(ingredientName));
        }
    }
</code></pre>

<ul>
<li>The custom repository inherits from the generic sharp architecture Repository class, ensuring that all the standard access methods are available </li>
<li>It implements the IIngredientRepository interface. This enables IoC injection of the repository into the controllers. </li>
<li>The FindByName method implements a <a href="http://nhforge.org/">Linq to NHibernate</a> query using the <i>Where</i> method. This will be transformed into a SQL query using a <i>Where</i> clause to prefilter the data before it is retrieved. </li>
</ul>

The custom repository method passes the unit test and returns the expected data. The original query is now optimised and will perform much better against larger data sets. I   ve improved the efficiency of the data access, however I   d also like to improve the functionality. So in the next post I will be looking at extending the ingredient repository with a new dynamic query. This will enable searching by an arbitrary number of keywords.<br />
