---
title: "Generate sitemaps using PowerShell"
date: "2010-01-15T02:40:54"
tags: [
  "powershell",
  "web"
]
---
![](Windows_PowerShell_icon_thumb.png)

I was discussing ‘**googlability**’  – a new word I made up meaning ‘the ability to find via Google’ – of our knowledgebase with one of the technical guys at work.  
It seems that we seldom get matches in Google searches (and the built in search is somewhat lame) – I was quite surprised with the fact that Google wasn’t matching anything.

Looking into it a bit further, I found that although our knowledgebase is public, the Urls are pretty undiscoverable, all having a ‘articleid’ parameter – obviously, the GoogleBot couldn’t just guess at the values and so was skipping the majority of our article, apart from the few listed on the main page.

We needed to give it some hints by adding a sitemap. I (ever so) briefly toyed with adding a sitemap page to the knowledgebase website using the standard XML based sitemap protocol etc, but our site is written in PHP and I didn’t want to get bogged down in all that again…  
In a rare burst of being pragmatic and keeping things simple (as opposed to **\_way\_** over engineering a solution) I recalled that Google’s webmaster tools allow you to submit a text file as a sitemap with one Url per line.

I knew the format of the Url for our articles so it just required a bit of PowerShell to generate a bunch of lines containing Urls with sequential numbers and write them to a file. version 1 looked like this :

set-content "c:sitemap.txt" (1..1000 | %{ "http://support.c2c.com/index.php?\_m=knowledgebase&\_a=viewarticle&kbarticleid=$\_&nav=0\`n" })  

However, uploading this sitemap caused the Google machine to choke and spew out a bunch of errors about invalid Urls… A little more digging uncovered that the text file uploaded must be encoded in UTF8. So version 2 looked like this :

set-content "c:sitemap.txt" (1..1000 | %{ "http://support.c2c.com/index.php?\_m=knowledgebase&\_a=viewarticle&kbarticleid=$\_&nav=0\`n" }) -encoding UTF8  

Out popped a text file with 1000 Urls, in the correct format, with the correct encoding and accepted by the Google machine with no problems.  
Probably 10 minute work all in – I wouldn’t have even got the PHP coding tools fired up in that time – reminder to self “***KISS works !!***”

GEO51.4043502807617:\-1.28752994537354