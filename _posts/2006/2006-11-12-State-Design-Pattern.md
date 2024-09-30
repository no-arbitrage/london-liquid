---
title: "State Design Pattern"
date: "2006-11-12T15:26:18"
tags: [
  "development"
]
---
In the first version of my GPS Library (written in VB6) I had a big horrible multiple nested if statement that handled the intelligence behind what to do when a new frame of data was received – if we were sending Waypoints to the GPS and the frame was an Acknowledgement (ACK) then we would send the next one, if it was a Not Acknowledgement (NACK) then we’d retransmit the last one – everything that happened depending on the current context (what we are currently doing). As there were about 20 different things we could be doing at the time and about 20 odd frame types, this was real messy and probably had a Cyclometric Complexity of 9292736592 (or more)…

My .NET version of the GPS Library makes extensive use of Software Design Patterns – to solve the problem above I used a ‘State’ pattern.

The State pattern is used where your software has multiple states and various actions or stimulations that make it change between states (but it is always going to be in one of the predefined states). I guess it a pretty common pattern for communication systems.

I have a base class called GpsState, that contains all an (overrideable) method for each action and stimulation and each method has a default implementation of something like

> throw new Exception(“Invalid action for state :” + this.StateName);

All the state classes inherit from this base state class.

Lets take (much simplified) sending Waypoints as the example:-

The GPS object has a reference to the current state class. This starts off in the stateIdle (doing nothing). We then call the stateIdle.WriteWaypoints() function. In this function we send the first Waypoint and change the state to stateWritingWaypoints.  
The stateWritingWaypoints overrides the ‘GotACK’ method and provides it own implementation (which is simply to send the next Waypoint). If any of the other methods are called whilst in the stateWritingWaypoints then the implementation from the base class is called and the caller receives an exception.  
When I’ve sent the last Waypoint then I change state back to stateIdle so that other actions can be called again

So my class for stateWritingWaypoints handles writing Waypoints and nothing else – this makes the code really clean and simple. The downside is that you end up with many more (small) classes in your code.

Watch this space for the source to the free .NET GPS Library.