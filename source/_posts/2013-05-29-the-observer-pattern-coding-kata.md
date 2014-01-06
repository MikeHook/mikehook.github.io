---
layout: post
title: The Observer pattern coding kata
categories:
- Software Design
tags:
- Design patterns
- Kata
status: publish
type: post
published: true
meta:
  _wpcom_is_markdown: '1'
  _wpas_done_327858: '1'
  publicize_twitter_user: michael_hook
  _wpas_done_327859: '1'
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:33610861;b:1;}}
  _elasticsearch_indexed_on: '2013-05-29 07:53:06'
---
The second design pattern I will be looking at is the Observer pattern, which is useful for safely passing data between objects. The Observer pattern is defined as:

<blockquote>"a one-to-many relationship between objects so that when one object changes state all it  dependants  are notified and updated automatically".</blockquote>

A good analogy for this pattern, described in the excellent <a href="http://www.headfirstlabs.com/books/hfdp/">Head First Design Patterns book</a>  is that of a magazine publisher and subscribers. Here the publisher is the <em>one</em>  in the relationship and the subscribers are the <em>many</em>. Typically the publisher will notify each of the subscribers of a new magazine edition by sending them the magazine in the post or through their e-book subscription.  Translated into the Observer pattern the Publisher is known as the Subject and the subscribers are the observers. The subject <em>notifies</em> the observers of changes in its state.

Some important features to note about this pattern are:

<ul>
    <li>Observers can not change the state of the subject and vice versa (ie. they are Loosely Coupled)</li>
    <li>The state information can be either <em>pushed</em> out to the observers by the subject or <em>pulled</em> from the subject by the observers</li>
    <li>Observers can be added and removed at any time</li>
</ul>

<h3>The brief</h3>

This is our  fictitious  brief for a new system which needs to be designed. It is election time and the polls are coming in. A local TV station would like us to design a system which can keep track of the results as they arrive. There are several hundred constituencies (areas of the country) which may declare for the Blue, Red, Yellow or Green party. The TV station wants to present this data in a number of ways:

<ul>
    <li>As a leader board, showing the tally for each party</li>
    <li>As a map, with each region coloured to the winning party</li>
    <li>As a <a href="http://news.bbc.co.uk/1/hi/uk_politics/election_2010/8574653.stm">Swingometer</a>  showing the proportional change and overall result</li>
</ul>

We don't need to worry about the implementation details of these display methods, the algorithms will be provided by the TV station. Our main concern is how to design a system to pass the data between the election object and the display objects.

<h3>Designing the Solution</h3>

Here is the class diagram for our solution, if you would like to try implementing the solution yourself the starting point for the kata is tagged here: <a href="https://github.com/MikeHook/DesignPatterns/tree/ObserverStart">Observer Pattern Kata Start</a>.

<img class="alignnone" alt="Observer pattern for election data" src="https://www.lucidchart.com/publicSegments/view/51892f26-05fc-4844-a25b-4c660a0087f6/image.png" width="579" height="597" />

We have leveraged the Observer pattern in our solution. The Election object inherits from ISubject and each of the display methods inherit from IObserver. The Election object maintains a list of the observers, we can add or remove an observer to this list using the <em>Register</em> and <em>Unregister</em> methods. When the subjects <em>Notify</em> method is called it informs each of the observers that a change has occurred by calling their <em>Update</em> method. A completed implementation for the election scenario is tagged here: <a href="https://github.com/MikeHook/DesignPatterns/tree/ObserverEnd">Observer Pattern Kata End</a>.
