---
title: "Dasblog Macros"
date: "2006-10-03T02:26:44"
tags: [
  "dasblog",
  "development"
]
---
So, having recently updated this website to Dasblog 1.9, I decided it was time to check out the source and have a play around.

The initial easy step to get involved seemed to be writing some Macros (I wanted to extend the default blogStats() macro to include Total Trackbacks and also add a Trackback Count to each post item in the footer.

There is a great intro to [Creating Dasblog Macros](http://dasblog.info/CategoryView,category,Development%2cCreating%2BMacros.aspx) on their Documentation Site, also [googling for Dasblog macros](http://www.google.co.uk/search?hl=en&rls=GGIC%2CGGIC%3A2006-37%2CGGIC%3Aen&q=dasblog+macros&btnG=Search&meta=) uncovered a few good sites

Anyway, I cracked open VS and spent a couple of hours writing some new macros, compiled them, made the requisite changes to my homeTemplate and itemTemplate but they just seemed to be return null strings. Checking the (Dasblog) event log showed that it was failing the find the Type for my Macros.[![](Capture_10-03-2006_1_thumb%5B2%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/DasblogMacros_2C6D/Capture_10-03-2006_1%5B2%5D.png)

I checked and rechecked everything and was convinced that everything was as it should be. Stumped!!  
Viewed my DLL in reflector, typenames were all correct – what was I doing wrong ??

A quick post to the Dasblog developers mailing list resulted in someone suggesting that I check if I was running a .NET2 compiled DLL in a .Net1.1 IIS virtual directory – Doh !!

Updated the virtual directory to ASP.NET 2.0 (easier than reverting to VS2003 from VS2005), ‘touched’ the web.config, refreshed the page and *‘ta da…’*

Changes and source code to be rolled out to this web site in the next day or so (more testing to do on how ASP.NET 2.0 works out), [![](Capture_10-03-2006_6_thumb%5B2%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/DasblogMacros_2C6D/Capture_10-03-2006_6%5B2%5D.png)