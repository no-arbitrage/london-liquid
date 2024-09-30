---
title: "Why blogging from Word 2007 to Dasblog does not work."
date: "2006-12-13T13:01:12"
tags: [
  "dasblog",
  "tools"
]
---
I was really excited by the [Blogging feature Word 2007](https://kapie.com/2006/12/04/Blogging+From+Word+2007.aspx) – It looked really good, easy to use and some of the formats you can do to a picture are pretty cool.

As soon as I found it I tried to set it up with this dasBlog account – Word 2007 is supposed to support the [metaWebLog API](http://www.xmlrpc.com/metaWeblogApi) (which dasBlog supports from v1.9), so in theory it should have worked – however it didn’t work fully.  
I could upload new blog posts but it didn’t like the posting of images – in returned a message saying it was not supported and I should use FTP upload instead.  
I wasn’t happy with this as the [metaWebLog API](http://www.xmlrpc.com/metaWeblogApi) has a newMediaObject method which allows for uploading of images etc and I know it is implemented in dasBlog (I think [Omar Shahine added it](http://www.shahine.com/omar/WindowsLiveWriter.aspx)).

[![](WordMetaWebLogError_thumb%5B2%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/TestPostfromWriter_A482/WordMetaWebLogError%5B6%5D.png)

Anyway, I emailed [Joe Friend](http://blogs.msdn.com/joe_friend/) (the MS guy who originally posted about this feature of Word) – he has since moved on so couldn’t help (in fact I think my email to him was the catalyst for his [Email is now off](http://blogs.msdn.com/joe_friend/archive/2006/12/07/email-is-now-off.aspx) post – oops)…

The annoying thing is that [Windows Live Writer](http://windowslivewriter.spaces.live.com/) supports the [metaWebLog API](http://www.xmlrpc.com/metaWeblogApi) implemented by dasBlog and can upload images fine….

Mmmmm, something not right here – taking things into my own hands, I cracked open [Fiddler](http://www.fiddlertool.com/fiddler/) and compared the HTTP request / responses between a [Writer](http://windowslivewriter.spaces.live.com/) posting and a Word posting. The Word postings always fail on the newMediaObject calls – here is a screenshot of the request / response from Word.

Looks like Word is trying to pass the blogid parameter as an int and not as a string as the [spec](http://www.xmlrpc.com/metaWeblogApi) dictates…

As far as I know, Word works correctly with MySpaces, Blogger and a few other blogging engines – maybe they are not as strict as dasBlog when checking the parameter types passed ??

I suppose I could write an HTTPHandler or something to convert that type description from an ‘int’ to a ‘string’.

[monstersessay.com](http://monstersessay.com/write-my-essay/)