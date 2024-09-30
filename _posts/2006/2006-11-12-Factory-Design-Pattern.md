---
title: "Factory Design Pattern"
date: "2006-11-12T15:57:31"
tags: [
  "development"
]
---
In the first version of my GPS Library software (written in VB6) I had a ‘Protocol’ property that indicated which protocol we were using (for example Garmin, Magellan, NMEA etc). Then, every time we needed to do something at the protocol layer (Sending or Receiving data) we had an if statement :

If gps.Protocol = GpsProtocol.Garmin Then  
     ‘ Do it this way  
EsleIf gps.Protocol = GpsProtocol.Magellan Then  
     ‘ Do it that way  
Else  
     ‘ Do it the other way

Horrible and messy !! Adding a new protocol meant going back to every function and amending it.

In the new .NET version, it’s much more object oriented and I’ve used a number of software design patterns.

To solve the above problem I used a Factory pattern. The Factory pattern allows you to create objects but let the client decide what type those objects should be. So, I can basically let the client set the Protocol property and then, depending on that setting create a different object for each protocol.

I start with a base class that all the ‘codec’ classes inherit from, then in the ‘creation’ (when the client creates the object) I look at the Protocol property and choose which concrete class to return:

GpsCodec is the base class and the concrete classes are GarminCodec, MagellanCodec etc. I have a static method in the base class called ‘CreateCodec’ that takes a ‘Protocol’ parameter. So it looks like this

public static GpsCodec CreateCodec(GpsProtocol protocol)  
{  
     switch (protocol)  
     {  
          case GpsProtocol.Garmin:  
               return new GarminCodec();  
               break;  
          case GpsProtocol.Magellan:  
               return new MagellanCodec();  
               break;  
          default:  
               return null; // error  
               break;  
     }  
}

So now I can simply create a codec class based on the Protocol property and use that for all the communication to / from the device. Adding a new protocol is simply a case of creating the new class and modifying the CreateCodec method – much simpler.

Watch this space for the free .NET GPS Library (including source).