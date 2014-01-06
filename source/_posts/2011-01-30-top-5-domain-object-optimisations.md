---
layout: post
title: Top 5 domain object optimisations
categories:
- Software Design
tags:
- DDD
- NHibernate
status: publish
type: post
published: true
meta:
  _wpas_done_twitter: '1'
  _elasticsearch_indexed_on: '2011-01-30 21:12:57'
  _wpcom_is_markdown: '1'
---
<div>I've been working with domain objects for a while now and these are simple tips I automatically follow on new objects. In my case the domain objects are setup to be mapped to a database by NHibernate, however most of these optimisations would be valid for a domain object used with any mapping tool.   I've found that these changes are quick to implement but can save a lot of hassle in the long run!</div>

<h3>1. Make the parameterless constructor private</h3>

Obviously you will need a public constructor as well, however this should take parameters. These should be the minimum set of properties that make the domain object valid. In this way you can be sure that whenever you encounter one of these objects in your code it will not have random null properties and cause unexpected null references.

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:60px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>private Customer() {}

public Customer(string firstName, string lastName) {}
</code></pre>

<h3>2. Add precondition checks in the constructor</h3>

The main advantage of this is to ensure that the constructor is used as intended. The precondition checks can be used to make sure that no null or empty parameters are passed into the constructor. When used in tandem with the first tip, you can be sure that you will not encounter any invalid domain objects in your code.

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:80px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>public Customer(string firstName, string lastName)
{
   Check.Require(string.IsNullOrEmpty(lastName) == false,
      "lastName must be provided");
}</code></pre>

<h3>3. Initialise your lists properties in the constructor</h3>

This particular change may only be needed for NHibernate. A list property can be used to represent a one-to-many relationship between to different classes. For example, typically a  <em><strong>Customer </strong></em>object could be related to multiple  <em><strong>Order </strong></em>objects, so this would be represented in the Customer class as a list of Order objects. However, if the customer had not placed any orders yet and you tried to access the orders list, without first initialising it, you would get a null reference.

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:60px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>private Customer()
{
   Orders = new List();
}</code></pre>

You will not get this problem if the list is always initialised in the constructor. NHibernate populates the list after the constructor has been called so no actual data will be lost when the list is initialised.

<h3>4. Make any Status properties private</h3>

A convention we commonly use with domain objects is to represent its state with an enum property. For example an order could be New, Pending or Complete. If the Status property is private then it can only be changed by Methods in the domain object itself. This has two main advantages, firstly any logic that needs to be associated with the status change can be included in the same method. In this way you can be sure that all the required logic is run when the status is changed. For example, you may need to set the order completion date when the status changes from Pending to Complete.   Secondly it makes the code much easier to re-factor in future as there will not be calls to set the property spread around the code base.

<pre style="background-color:#eeeeee;border:1px dashed #999999;color:black;font-family:andale mono, lucida console, monaco, fixed, monospace;font-size:12px;height:100px;line-height:14px;overflow:auto;width:100%;padding:5px;"><code>OrderStatus Status {get; private set; }

public CompleteOrder()
{
   Status = OrderStatus.Complete;
   CompletedDate = DateTime.Now();
}</code></pre>

<h3>5. Keep it simple!</h3>

OK I admit it, I ran out of tips after four! But I think this is a valid point. There is nothing worse than discovering a domain object of 500 plus lines of code in a project you need to maintain. It is best to keep it simple and if you find that too much behaviour is associated with one object then either split it into smaller domain objects or move the behaviour into a separate service class.

So those are my top 5 tips for domain objects. I'd be interested to hear opinions on these, along with  practices  that anyone else employs for domain objects so feel free to leave comments.
