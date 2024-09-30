---
title: "Insert Geo Microformat Plugin for Windows Live Writer"
date: "2007-09-15T22:12:11"
tags: [
  "dasblog",
  "web"
]
---
[![InsertPanel](InsertPanel_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/InsertGeoMicroformatPluginforWindowsLive_13B9D/InsertPanel.png) I have just completed a new [Windows Live Writer](http://windowslivewriter.spaces.live.com/) plugin. This extension allows ease insertion of [geo microformat](http://microformats.org/wiki/geo) information.

It allows the user to easily choose the location they want to insert (in microformat) from a [Virtual Earth](http://local.live.com/) map and also configure how it is displayed (if at all on the post.

Recently I (with considerable help from [Alexander Groß](http://www.therightstuff.de/About.aspx)) [added GeoRss support](https://kapie.com/2007/09/08/GeoRss+For+DasBlog.aspx) for [dasBlog](http://www.dasblog.info). The co-ordinates can be specified when adding a post via the web interface. This plugin is stage two of this support, stage three will be parsing the geo microformat when a post is added and using that to populate the GeoRss info.

The end goal being to allow the geo info to be entered when creating a post in Writer and having that info available in GeoRss format in the feed.

I started this plugin with the view to using [Google Maps](http://maps.google.com/), however they require that you get an API key and that key is only valid for a particular web site / URL path. This foiled my plans to embed the map in a Windows Forms WebBrowser control (I did look at producing an html page that was served from my web site, and using it embedded in the WebBrowser – not scaleable and too much configuration for a normal user to do.

[![InsertMicroformat](InsertMicroformat_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/InsertGeoMicroformatPluginforWindowsLive_13B9D/InsertMicroformat.png) 

I hadn’t looked in depth at [Virtual Earth](http://local.live.com/), but recently went to [MixUK:07](http://www.microsoft.com/uk/mix07/) and saw a couple of demos / presentations on it – a quick look found that it didn’t need a API and was not tied to a particular URL / path – the JavaScript for it is pretty similar to the one for [Google Maps](http://maps.google.com/) so learning curve was pretty short. The only issue I have with [http://local.live.com](http://local.live.com) is that there is no (currently) facility to enter a GeoRss feed in the search query and just display the data (the current method of displaying this kind of data is to embed a map in your own pages and use their API to display the items as a ‘collection’).

You can get the installer for it here [InsertGeoFormatSetup.msi (325Kb)](https://kapie.com/content/binary/insertgeoformatsetup.msi).

GEO 51.4043243116043:\-1.28760516643523