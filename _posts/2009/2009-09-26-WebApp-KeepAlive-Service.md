---
title: "WebApp KeepAlive Service"
date: "2009-09-26T21:30:41"
tags: [
  "development",
  "web"
]
---
[![image](image_8.png "image")](http://www.dotnetnuke.com) Recently I have been working with [DotNetNuke](http://www.dotnetnuke.com). This is a superb open source CMS platform running on ASP.NET with a SQL back end, simple to install, easy to use and there is a thriving community around it. It is also available in a ‘Professional’ version which costs around £2000 per year and provides additional workflow features, a support contract and various other benefits.

[![image](image_9.png "image")](http://www.datasprings.com) I choose not to go for the pro version, just because I don’t see much point in having some aspects of a website/webapp covered with a support agreement, but not others (any third party controls/extensions are not covered, and I had bought the excellent [DataSprings](http://www.datasprings.com) Suite and was using multiple components from it).

Anyway, when I had everything working the way I wanted, I started to look at performance. There are many websites providing tools and tips around maximizing performance of DNN sites – this includes caching strategies, output compression, turning off none essential ‘housekeeping tasks’ and the like.  
One of the tweaks (for low traffic sites) was to ‘kick’ the site every few minutes to ensure that IIS does not unload it (and therefore need to spin it up again the next time someone visits – this can take a good few seconds).[![image](image_thumb_4.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/WebAppKeepAliveService_E973/image_13.png)

  My initial thoughts were around a scheduled PowerShell command – simple to come up a one liner to request a page and thus keep the web app in memory.

(new-object “System.Net.WebClient”).DownloadString(“http://www.website.com/”)

Then I thought I might have it ‘kick’ a few websites, and make it configurable, so I started thinking ‘Windows Service’. It turns out that there are a load of these apps and services available to buy, some targeted specifically at DNN, some more generic. Reluctant to spend $20/year on a service I decided to craft my own.

It is basically a windows service, that reads an XML file for a timeout and a list of urls to kick. It implements a 1 second timer and counts down the timeout value, when it reaches 0 it kicks each of the urls (recording how long the response took). I haven’t done anything with the measured response time, but it would be fairly easy to write out to a DB or file for later analysis…

Here is a .zip file with everything you need – binaries, sample configuration file, install instructions and full source. It is Creative Commons license so knock yourself out.

[![WebAppKeepAlive.zip](image_7.png "WebAppKeepAlive.zip")](https://kapie.com/projects/webappkeepalive/webappkeepalive.zip)

GEO 51.4043197631836:\-1.28760504722595