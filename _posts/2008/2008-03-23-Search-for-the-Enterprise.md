---
title: "Search for the Enterprise"
date: "2008-03-23T11:21:13"
tags: [
  "technical"
]
---
It has been a real busy month at work and I’ve just not had a minute.

[![screenshot_startMenu_Search](screenshot_startMenu_Search_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/SearchfortheEnterprise_9F1E/screenshot_startMenu_Search_2.jpg)One of the big things I see on the (technical) horizon is ‘federated search’ for the Enterprise. We already have this for the desktop / individual in the form of [Google Desktop Search](http://desktop.google.com/en/GB/?utm_source=en_GB-et-more&utm_medium=et&utm_campaign=en_GB), [Vista ‘Instant Search’](http://www.microsoft.com/windows/products/windowsvista/features/details/instantsearch.mspx) and many others.

Google have already brought some of this to the enterprise with their [Enterprise Edition](http://desktop.google.com/en/GB/enterprise/index.html) (but I’ve heard horror stories about getting this installed and working correctly)…

Microsoft are entering this space with their [‘Search Server’](http://www.microsoft.com/enterprisesearch/) (also available in a free [Express](http://www.microsoft.com/enterprisesearch/serverproducts/searchserverexpress/default.aspx) version).[![gds](gds_thumb.gif)](https://kapie.com/content/binary/WindowsLiveWriter/SearchfortheEnterprise_9F1E/gds_2.gif)

There are two key concepts here :-

-   Federated Search Providers 
-   Search Connectors

At work we have added support for a Federated Search Provider to Archive One and will be adding Search Connectors for Search Server (and others) shortly, and that has resulted in me having to explain the difference between them and write a bunch of marketing stuff around it.

A **Federated Search Provider** is a facility for the search technology to pass your application / solution a search query that is then executed by your application / solution and the results sent back to the search technology.

> *Example*:
> 
> -   A user goes to the Search Server interface and enters the search query ***enron shredding*** (why does Enron always get cited in our industry ??).
> -   Search Server looks through the index of data it knows about (files, Exchange data, SharePoint data etc) and gets a set of results.
> -   Search Server also passes the query (***enron shredding***) to the Archive One Federated Search Connector.
> -   Archive One searches through the archived email and gets a set of results.
> -   The Archive One results are passed back to Search Server.
> -   Search Server combines both sets of results and displays them to the user.

The key behind this is that the Search Server knows nothing about the Archive One and the search is actually carried out by Archive One *on behalf of Search Server.  
*The benefit is that there is no dependance on Search Server having to index the data itself so as soon as Archive One has the data it is searchable.

A **Search Connector** is a facility for Search Server to ‘crawl’ the Archive One data and index the data itself. All search results are returned by Search Server directly.

> Example:
> 
> -   Over time Search Server crawls the Archive One data and creates it’s own index of that data.
> -   A user goes to the Search Server interface and enters the search query ***enron shredding.***
> -   Search Server looks through the index of data it knows about (files, Exchange data, SharePoint data and ***Archive One data***) and gets a set of results.
> -   Search Server displays them to the user.

The key behind this method is that Search Server also indexes all the Archive One data and it knows all about it without having to farm out the query to another location.

[![subhero_searchserver](subhero_searchserver_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/SearchfortheEnterprise_9F1E/subhero_searchserver_2.jpg)  
Other advantages here are that supporting a Federated Search Provider (and the OpenSearch V1.1 protocol) allows you to add the site as a ‘Custom Search Provider’ in IE7, supporting Search Connectors for one vendor / technology allows support of the many other vendors / technologies with just a little extra tweaking.

I have always said that customers “don’t want islands of data, they want connected, synergistic solutions” and these technologies are going to make that happen, gone are the days of hunting around file shares, local disks, Outlook etc for that elusive PowerPoint presentation. I can search across the whole enterprise from one simple location. Imagine searching for all communication with customer X and getting results from everyone email, archived email, MSCRM, SharePoint, saved IM conversations, VoIP audio streams, scanned faxes, Exchange Unified Messaging voicemails. ***Never lose anything.***

GEO 51.4043197631836:\-1.28760504722595