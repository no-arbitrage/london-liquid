---
title: "Activity RSS for dasBlog"
date: "2007-07-20T23:18:41"
tags: [
  "dasblog"
]
---
A few months ago I added a ‘Daily Report Email’ feature to dasBlog, so I could have the activity reports (referrers, searches etc) delivered to my mailbox at the end of every day.  
I wanted to take this one step further and deliver it via RSS (and dasBlog ***is*** a RSS engine after all).

There were a couple of suggested methods raised on the [dasBlog developers mailing list](https://lists.sourceforge.net/lists/listinfo/dasblogce-developers); one was using HTTP authentication – I was reluctant to use this as Outlook 2007 does not support authorized feeds. The other suggestion was to obfuscate the URL with a Guid or the like – this was my preferred option.

So, I have been working on it over the past few evenings, got it rolled out to this blog and it seems to be working pretty well.

![image](image_thumb.png)

I added a checkbox to the site configuration page, that when checked generates a Guid that is used as part of the path. The ‘activityrss.ashx’ page doesn’t exist at that path (in fact it doesn’t exist at all!), instead there is a HttpHandler for the file ‘activityrss.ashx’ that intercepts any requests for that file and does some processing.

In this processing I check that the requested URL contains the Guid and if so, generate the Activity RSS feed. The content of each feed item is the activity report (similar format to the daily report email).

[](https://kapie.com/content/binary/WindowsLiveWriter/b80e199275a6_10B/image.png)

[![image](image1_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/b80e199275a6_10B/image1.png)

NOTE: This is not in a release yet, if you want it you’ll have to check out the source, [patch it with this patch file](https://kapie.com/content/binary/ActivityRSS.patch) and compile, deploy it manually.