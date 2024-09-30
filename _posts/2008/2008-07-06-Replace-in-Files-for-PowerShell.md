---
title: "Replace in Files for PowerShell"
date: "2008-07-06T14:35:38"
tags: [
  "dasblog",
  "powershell",
  "scripting",
  "web"
]
---
A while back I restructured my website so that this blog no longer started at the root, instead starting from /blog. This was so that I could introduce some other web apps and have a subfolder for projects etc.

One of the pains of this restructure was modifying all the links – I thought I had caught all this with a [Redirector HttpModule](https://kapie.com/2007/12/19/RedirectorHttpModule.aspx), but recently realised that for some reason I had not caught images embedded in the posts themselves.  
Also it was becoming a pain having to remember to include the HttpModule in my web.config everytime I upgraded my blog ([dasBlog](http://www.dasblog.info/))

I wanted it fixed properly this time, so grabbed a copy of all the XML files in my ‘content’ folder, copied them to a local folder and cracked open PowerShell…![](powershell_logo.jpg)

I wanted every instance of [www.mywebsite.com](http://www.mywebsite.com) changed to [www.mywebsite.com/blog](http://www.mywebsite.com/blog) – not difficult, but this would also change valid urls such as [www.mywebsite.com/blog/page.aspx](http://www.mywebsite.com/blog/page.aspx) to [www.mywebsite.com/blog/blog/page.aspx](http://www.mywebsite.com/blog/blog/page.aspx) (note the **/blog/blog** in the url)

So I got everything I needed done with two ‘one liners’ in PowerShell…

***dir | %{ $a = get-content $\_ ; $a = $a -replace (“www.mywebsite.com”, “www.mywebsite.com/blog”) ; set-content $\_ $a }***

…and…

***dir | %{ $a = get-content $\_ ; $a = $a -replace (“www.mywebsite.com/blog/blog”, “www.mywebsite.com/blog”) ; set-content $\_ $a }***

All fixed…

GEO 51.4043197631836:\-1.28760504722595