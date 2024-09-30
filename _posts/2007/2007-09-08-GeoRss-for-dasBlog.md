---
title: "GeoRss for dasBlog"
date: "2007-09-08T21:33:40"
tags: [
  "dasblog",
  "development"
]
---
I have completed ***stage one*** of GeoRss enabling dasBlog.

In the config page I added some options for enabling GeoRss, specifying a default lat/long and enabling ‘integration’ with [Google maps](http://maps.google.com/). There is also an option to use the default lat/long for any non geocoded posts.

  ![clip_image0022_thumb1](clip_image0022_thumb1_1.gif)

If GeoRss is enabled then the edit entry screen provides textboxes to allow specifying lat/long (populated with defaults from config page).

![clip_image00421_thumb2](clip_image00421_thumb2_1.gif)

If the [google maps](http://maps.google.com/) integration is enabled then you’ll get the ‘Show Map’ button and clicking it will display a map which you can move around until you find the location and then click on the location to get the lat/long texboxes populated.

![clip_image00681_thumb1](clip_image00681_thumb1_1.gif)

If you have existing non geocoded posts then you can have the default lat/long added to those if you wish.

I puzzled around for ages when trying to display the georss in google ([http://maps.google.com/maps?q=yourfeedaddress](http://maps.google.com/maps?q=yourfeedaddress)) – it kept telling me that the feed was invalid. I eventually found that [feedburner](http://www.feedburner.com) was adding <atom10:link blah blah /> to the xml which for some reason [google maps](http://maps.google.com/) thinks is invalid. The only way I could find to prevent [feedburner](http://www.feedburner.com) adding the atom link was to turn OFF the ‘Browser Friendly’ feature in [feedburner](http://www.feedburner.com).

So – stage 2…

The work I still want to do with this is basically to add macros to get lat/long  – fairly easy I guess, and then some way to specify lat/long from [Windows Live Writer](http://gallery.live.com/default.aspx?pl=8) (and other offline blog clients) – a little more complex. [Scott](http://www.hanselman.com/blog) mentioned a geo microformat and from my initial looks seems to be a good route to take – watch this space…  

Now it is simply a case of retrofitting the geo info into all my old posts…