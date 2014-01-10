---
layout: post
title: Design patterns without Katas
categories:
- Software Design
tags:
- Design patterns
- Kata
status: publish
type: post
published: true
date: 2013-06-13 08:05:05
meta:
  _wpcom_is_markdown: '1'
  _wpas_done_327858: '1'
  publicize_twitter_user: michael_hook
  _wpas_done_327859: '1'
  _publicize_done_external: a:1:{s:7:"twitter";a:1:{i:33610861;b:1;}}
  _elasticsearch_indexed_on: '2013-06-13 08:05:05'
  twitter_cards_summary_img_size: a:7:{i:0;i:960;i:1;i:642;i:2;i:2;i:3;s:24:"width="960"    height="642"";s:4:"bits";i:8;s:8:"channels";i:3;s:4:"mime";s:10:"image/jpeg";}
---
There are a number of design patterns which I have chosen not to implement a coding kata for. This is because I felt that I would either not use the pattern regularly in practice or they have alternative solutions that make the pattern redundant. However I think it is worth calling out these patterns and summarising how they work.

<h2>The decorator pattern</h2>

This pattern provides a way of extending a classes behaviour without using inheritance, it is defined as:

<blockquote>The decorator pattern extends the functionality of individual objects by wrapping them with one or more decorator classes. These decorators can modify existing members and add new methods and properties at run-time.</blockquote>

The main reason I won't be using this pattern much in practice is that the class structure it generates is not very clear and also the behaviour of   the classes can change depending on the order they are instantiated. So I think implementing this pattern could cause more problems than it solves, especially if the system may need to be maintained by different people at a later date.

<h2>The factory pattern</h2>

<blockquote><em>The factory method pattern allows for the creation of objects without specifying the type of object that is to be created in code. A factory class contains a method that allows determination of the created type at run-time.</em></blockquote>

Whilst the factory method has its uses it is basically just an implementation of sub classing, where parameters are defined as base classes to allow different derived classes to be passed at runtime. It is a straightforward pattern and the coding kata would be pretty short to implement it.

Also one of the main issues the factory pattern tries to solve is centralising object creation, however we generally use IoC containers which provide <a href="http://stackoverflow.com/questions/871405/why-do-i-need-an-ioc-container-as-opposed-to-straightforward-di-code">low maintenance dependency injection</a>  out of the box. So this removes the main need for the factory pattern.

<h2>The Singleton pattern</h2>

<blockquote>Ensure a class only has one instance and provide a global access point to it</blockquote>

<a href="http://bakingwebsites.files.wordpress.com/2013/06/asparagus-shoot.jpg"><img class="alignright size-medium wp-image-460" alt="asparagus shoot from http://sandyspringcsa.com/" src="http://bakingwebsites.files.wordpress.com/2013/06/asparagus-shoot.jpg?w=300" width="300" height="200" /></a>
This is a pattern which can be very useful in applications, however the implementation is so short that a coding kata would have little content. There are just a couple of pitfalls to watch out for when implementing this pattern:

<ul>
    <li><span style="line-height:16px;">Ensure the implementation is  thread safe
In Java the synchronized keyword can be used on the method which returns the singleton to ensure only a single thread ever has access to the class at once. However that can lead to the second pitfall</span></li>
    <li>Consider whether you implementation offers the best performance
Using synchronization is expensive (ie. It takes the computer a relatively long time to process)</li>
</ul>

This implementation avoids both of these pitfalls by creating the <em>uniqueInstance</em> when the class is first loaded:

<pre style="font-family:Andale Mono, Lucida Console, Monaco, fixed, monospace;color:#000000;background-color:#eee;font-size:12px;border:1px dashed #999999;line-height:14px;overflow:auto;width:100%;padding:5px;">public class Singleton {
  private static Singleton uniqueInstance = new Singleton();

  private Singleton() {}

  public static Singleton getInstance()  {
    return uniqueInstance;
  }
}</pre>

Finally, as with the factory pattern, if you are using an IoC container then they generally provide a way to configure a class as a singleton so there is rarely a need to implement this pattern manually yourself.

<h2>There may be more...</h2>

Stay tuned for more patterns to be added to this post!
