---
layout: post
title: Strategy pattern coding kata
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
  _wpas_skip_327859: '1'
  _wpas_skip_327858: '1'
  _elasticsearch_indexed_on: '2013-05-07 07:16:49'
---
I've been having a look at coding katas and design patterns over the last couple of weeks. If you are not familiar with either of these concepts then you can read <a href="http://butunclebob.com/ArticleS.UncleBob.TheBowlingGameKata">Uncle Bob's introduction to the Bowling Kata</a> and check out <a href="http://www.blackwasp.co.uk/GofPatterns.aspx">Richard Carr's great set of articles on design patterns</a>.

My main goals are to:

<ul>
    <li><span style="line-height:13px;">Increase my familiarity with design patterns</span></li>
    <li>Understand how design patterns can be best applied in real life situations</li>
    <li>Develop a repeatable method for memorizing the patterns</li>
</ul>

A good way to cover off all of these goals will be to develop a set of code kata's which can be solved by applying a design pattern.  Sounds simple? Maybe but lets get started and see how it goes!

<h2>The Strategy Pattern</h2>

The strategy pattern is defined as<em>   </em>

<blockquote><em>"a design pattern that allows a set of similar algorithms to be defined and encapsulated in their own classes." </em></blockquote>

The aim of this pattern is to separate the parts of a system that may change from those which are unlikely to change, allowing for easier maintainability of the system in future.  This all sounds good in theory but you may be wondering how and where the pattern can be applied in practice. With this in mind I've come up with a  fictitious brief that we can solve by utilising the Strategy Pattern.

<h4>The brief</h4>

A games company is developing a sports event simulator, to be built in iterations starting with simple requirements and building up to increase the complexity. The system design should allow for changes to be made by extending the existing system without changing what is already in place.

<ol>
    <li>The simulator must support marathon and 10 km run events. The only requirement for these events is that the competitors can be displayed in some way (text is fine) and they can compete in the event. All competitors will compete by running.</li>
    <li>After running some events they proved to be totally chaotic so each event should now include a marshal. The marshal can also be displayed but does not compete in events.</li>
    <li>Since the glorious victory of team GB in the Olympic Triathlon the sport's popularity has increased. So the games company would now like to also support triathlon events. Again competitors must be displayed and be able to compete, however a triathlon competitor will compete by swimming, cycling and running.</li>
</ol>

<h4>Designing the Solution</h4>

The starting point for this kata is available to clone from github here: <a href="https://github.com/MikeHook/DesignPatterns/tree/StrategyStart">Strategy Kata Start</a>  <a href="https://github.com/MikeHook/DesignPatterns/tree/StrategyStart">
</a>

<h5>Iteration 1</h5>

To meet the first requirement we only need a couple of objects as shown below:

<img class="alignnone" alt="Strategy Pattern - Design iteration 1" src="https://www.lucidchart.com/publicSegments/view/5180e984-cb84-4274-a415-68870a00005a/image.png" width="637" height="405" />

We can create a couple of events for marathon and 10km runs and add some runners to those events. When the simulation runs the Compete() base method is called for each of the EventAttendees to simulate them taking part in the event. Each of the runners can be displayed by a call to their <em>Render()</em> method.

<h5><span style="line-height:13px;">Iteration 2</span></h5>

We now need to include Marshals as part of the event, so the model is extended as below:

<img class="alignnone" alt="Strategy Pattern - Design iteration 2" src="https://www.lucidchart.com/publicSegments/view/518380c3-ab34-42c9-a96e-71dc0a005c5f/image.png" width="707" height="492" />

However, there is an obvious problem with this design, as noted in the diagram. One option would be for the Marshall to override the Compete method and make it do nothing. However this will store up more trouble for us as there may be other types of event attendee later which do not compete. We would have to ensure that each of these attendees also override that method with the same 'not compete' behaviour. There is a better way to design for this issue (there is a clue in the name of this blog post!)

<h5>Iteration 3</h5>

It is clear that the Compete behaviour may change frequently depending on the attendee, we can leverage the Strategy pattern to extract this behaviour into separate classes as shown below.

<img class="alignnone" alt="Strategy Pattern - Iteration 3" src="https://www.lucidchart.com/publicSegments/view/51877f21-1fd8-4c3b-9f02-04510a0087f6/image.png" width="519" height="496" />

The EventAttendee base class now contains a property of type ICompeteBehaviour. Any implementation of this interface can be assigned to each instance of an EventAttendee. So the <em>Runner</em> class is assigned the <em>Run </em>behaviour and the <em>Marshall</em> class is assigned the <em>DontCompete</em> behaviour.

Iteration 4

We have one more requirement to satisfy, the design needs to be extended to allow Triathlon events to take place. Thanks to our changes in the previous iteration we can add this new requirement without making any changes to the existing classes. Here is the final structure:

<img class="alignnone" alt="Strategy Pattern - Iteration 4" src="https://www.lucidchart.com/publicSegments/view/51878405-1770-4c88-b91a-761b0a000882/image.png" />

We have extended the <em>EventAttendee</em> class with a new <em>Triathlete</em> class and created a new <em>RunCycleSwim</em> implementation of the <em>ICompeteBehaviour</em> interface. You may be thinking that the same could have been achieved with a method for the behaviour on the Triathlete class, while this is true it limits the flexibility of the design. We can see the advantage of this design by adding Spectators to the simulation. They will also be assigned the DontCompete behaviour, so we have effectively shared this part of the logic without repeating the implementation.

The completed implementation for this kata is available on github here:  <a href="https://github.com/MikeHook/DesignPatterns/tree/StrategyEnd">Strategy Kata End</a>
