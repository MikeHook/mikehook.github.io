---
layout: post
title: Optimising data access queries (Part 2)
categories:
- Software Design
tags:
- Linq
- NHibernate
- sharp architecture
status: publish
type: post
published: true
date: 2009-10-16 12:55:00
comments: true
meta:
  blogger_blog: bakingwebsites.blogspot.com
  blogger_author: Mike Hook
  blogger_21e9281c9d3b6d2ff7500a6d800ca7bb_permalink: '7223163663595567406'
  _elasticsearch_indexed_on: '2009-10-16 12:55:00'
  _wpcom_is_markdown: '1'
---
In <a href="http://www.blogger.com/2009/10/optimising-data-access-queries-part-1.html">part 1</a> of this post I implemented a custom repository method to find all ingredients containing a keyword. The obvious improvement for this query was to extend the search to multiple keywords. As usual the first step was to write a unit test for the new method:

<pre style="border:1px dashed #999999;overflow:auto;background-color:#eeeeee;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:236px;line-height:14px;width:114.97%;padding:5px;"><code>        protected override void LoadTestData()
        {
            CreatePersistedIngredient(1, "Chicken thigh", FoodGroup.Meat);
            CreatePersistedIngredient(2, "Chicken breast", FoodGroup.Meat);
            CreatePersistedIngredient(3, "Red Pepper", FoodGroup.Vegetable);
            CreatePersistedIngredient(4, "Jalapeno Chilli", FoodGroup.Vegetable);
        }

        [Test]
        public void CanFindIngredientsByKeywords()
        {
            List&lt;Ingredient&gt; ingredients = ingredientRepository.FindByKeywords(new string[] { "chilli", "pepper" }).ToList();
            ingredients.ShouldNotBeNull();
            ingredients.Count.ShouldEqual(2);
        }
</code></pre>

The test expects 2 ingredients to be returned for the search     both the    Red pepper    and    Jalapeno Chilli    ingredients should be returned.

<h4>Predicate functions and Expression trees</h4>

I wanted to keep using the Linq-to-NHibernate library for the data access queries as this ensures that the queries inherit all the <a href="http://weblogs.asp.net/scottgu/archive/2006/05/14/Using-LINQ-with-ASP.NET-_2800_Part-1_2900_.aspx">benefits of Linq</a>. I also find it more intuitive to use than HQL, for simple queries at least! The core problem was how to write a Linq query that could be extended for an arbitrary number of keywords. In HQL this could be achieved by looping over the keywords and building up the query string. However it is not so easy to iteratively build a lambda function.

My first effort involved chaining together <a href="http://www.codeproject.com/KB/cs/FunWithFunc1.aspx">predicate functions</a>. However after passing the chained predicate to NHibernate the rather less than impressive result was nothing    for any query! Looking at the SQL query produced showed that an incomplete statement was being passed without any of the filter keywords. I was unable to find any documentation to speak of for Linq-to-NHibernate but the <a href="http://groups.google.com/group/nhusers">NHibernate user group</a> did prove useful. They suggested that I may have more success building up an <a href="http://blogs.msdn.com/charlie/archive/2008/01/31/expression-tree-basics.aspx">expression tree</a> and using that as the predicate.
<a href="http://blogs.msdn.com/charlie/archive/2008/01/31/expression-tree-basics.aspx"></a>

<h4>Dynamic Linq-to-NHibernate Query</h4>

After reading up on expression trees I was in a position to code up a method to dynamically build the predicate. While researching the best way to tackle this, the internet again reminded me that there are not many original problems left in this world! This article by Joseph Albahari on  <a href="http://www.albahari.com/nutshell/predicatebuilder.aspx">dynamically composing expression predicates</a> demonstrated how a helper class could be used to farm out the work of building up the predicate. The following class makes use of the PredicateBuilder class provided in the article:

<pre style="border:1px dashed #999999;overflow:auto;background-color:#eeeeee;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:180px;line-height:14px;width:114.82%;padding:5px;"><code>        public IEnumerable&lt;Ingredient&gt; FindByKeywords(IEnumerable&lt;string&gt; keywords)
        {
            Expression&lt;Func&lt;Ingredient, bool&gt;&gt; predicate = PredicateBuilder.False&lt;Ingredient&gt;();

            foreach (string keyword in keywords)
            {
                string temp = keyword;
                predicate = predicate.Or(i =&gt; i.Name.Contains(temp));
            }
            return Session.Linq&lt;Ingredient&gt;().Where(predicate);
        }
</code></pre>

<ul>
    <li> The expression predicate is initialised to false</li>
    <li>The predicate is chained together using the <em>Or</em> method of predicate builder class</li>
    <li>The final predicate will return true for any Ingredient that has a Name containing any of the keywords.</li>
</ul>

The FindByKeywords method passes the unit test at the start of the post. Implementing this method took a while, mostly due to the time taken to read up on the predicate functions and expression trees. However I think it was time worth spending as these language features are here to stay and will most likely increase in use over time. Also I now have a handy way of writing efficient Linq queries with multiple filter parameters :)