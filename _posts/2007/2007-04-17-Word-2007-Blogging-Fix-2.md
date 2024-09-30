---
title: "Word 2007 Blogging Fix 2"
date: "2007-04-17T17:57:45"
tags: [
  "dasblog"
]
---
One of the developers on the dasBlog team ([Nick](http://www.thecodingmonkey.net/)) looked at this and found that there was a fairly easy fix we could do to dasBlog that would get around the Word 2007 issue.

The metaWeblog API states that the blogid parameter is ‘***the same as defined by the blogger API’  
***The blogger API defines it as :

***blogid (string):** Unique identifier of the blog the post will be added to.*

As dasBlog is a single blog instance there is no real need for the blogid – we could simply pass back “0” which would allow Word 2007 to send us a blogid of “0” (which we don’t even use anyway).  
A fix (thanks [Nick](http://www.thecodingmonkey.net/)) is now in the latest source.

This post was published from Word 2007 and the $64000 question is – does it display this image ?

![](041707_1755_1.png)