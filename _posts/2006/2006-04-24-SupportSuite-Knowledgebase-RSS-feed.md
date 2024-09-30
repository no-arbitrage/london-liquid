---
title: "SupportSuite Knowledgebase RSS feed"
date: "2006-04-24T11:31:49"
tags: [
  "scripting",
  "technical"
]
---
I logged a feature enhancement to allow the KB article RSS feed to include articles from ALL categories.

This was set as a LOW PRIORIY by Kayako, but is a high priority for me – nothing else for it, I had to fix it myself…..  
Here’s how I added the feature to enter 0 as the category id in the RSS feed url to provide articles from ALL categories.

Edit **/modules/knowledgebase/functions\_clientkb.php**

Change Line 167 from :  
if ($key == $kbcategoryid)

To :  
if (($key == $kbcategoryid) || ($kbcategoryid == 0))

The url for the feed includes the category id for the artciles you want to see. Enter a category id of 0 to return ALL articles :

[http://\[your\_domain\]/rss/index.php?\_m=knowledgebase&\_a=view&kbcategoryid=0](http://[your_domain]/rss/index.php?_m=knowledgebase&_a=view&kbcategoryid=0)

[Link](http://forums.kayako.com/showthread.php?t=7329 "Kayako Forum post") to the post I entered on the Kayako forums

[scholarship essay writing](http://essayyoda.com/write-my-essay/)