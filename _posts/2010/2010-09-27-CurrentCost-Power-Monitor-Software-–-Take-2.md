---
title: "CurrentCost Power Monitor Software – Take 2"
date: "2010-09-27T20:30:18"
tags: [
  "development",
  "hardware",
  "featured"
]
image: /assets/images/currentcost-power-monitor-software-cc128-large_01_thumb.jpg
---
![cc128-large_01](/assets/images/currentcost-power-monitor-software-cc128-large_01_thumb.jpg)

This post is an update to my previous one about the CurrentCost Energy Monitor and the software I built for it.

After [posting brief details](https://kapie.com/2010/05/11/CurrentCostPowerMonitorSoftware.aspx) about it, I agreed with the guys over at the [MSDN Coding for Fun](http://blogs.msdn.com/b/coding4fun/) site to write a full project article  for them. The suggested a couple of changes – using the Managed Extension Framework (MEF) stuff available in .NET 4 and a Windows Phone 7 client, to make it more generally appealing.

I had not seen the MEF stuff before, but it turned out great – very simple framework that allows a full ‘plugin’ style architecture in around 10 lines of code. I also had little experience with the Xaml stuff required for WinPhone 7 apps, but it is actually pretty simple.

There are a number of plugins and clients for it now including :

-   A plugin to post to Twitter every X minutes (using oAuth)
-   A plugin to post to [Pachube](http://www.pachube.com) streams
-   A plugin to post to a web service
-   A web page to display the latest readings sent to the web service
-   A Windows Phone 7 application to display the latest reading sent to the web service
-   A Windows Sidebar Gadget to display the latest reading sent to the web service

I have also had submissions from a couple of other developers, one for a plugin that posts to [Google PowerMeter](http://www.google.com/powermeter) and another that records the readings in a database – both of these will be uploaded to the [Codeplex](http://energymonitor.codeplex.com/) site when I have them integrated into the source and working correctly.

Anyway, you can read all about it [here](http://blogs.msdn.com/b/coding4fun/archive/2010/09/27/10068304.aspx). There is also a [Codeplex](http://energymonitor.codeplex.com/) site for all the source code and binaries.

Post in the comments if you have suggestions for other plugins or features, or want to get involved.
