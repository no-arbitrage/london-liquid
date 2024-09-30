---
title: "Writing Supportable Software 1 – Tracing for maximum effect"
date: "2006-10-27T00:47:14"
tags: [
  "development"
]
---
At work ([C2C](http://www.c2c.com/)) I’m responsible for the development team and the technical support team and it proves to be a surprisingly difficult task maintaining a balance between their respective objectives – the two extremes are :

-   Support has little or no contact with development, bug escalations are done in a very formal way and the overall drive is to ensure development are not distracted by support work.
-   Support has unhindered access to development, can draw on their expertise / knowledge whenever they like, the overall drive being to have happy customers (via quick and accurate resolutions)

***Neither*** of these work well in a business – with the first you sacrifice customer satisfaction for development productivity and in the second it’s the other way around… The key is finding the balance, but it’s tough finding that balance, and there is no one answer or simple formula – it depends on many factors where (between the two extremes) you find the balance (product quality, support team knowledge, customer expectations etc)

One thing that helps is getting the development team is to write *Supportable Software* – this means developing it in such a way that we maximize the troubleshooting efforts that can be carried out *before* it has to be escalated to the development team. There are a number of facets to this, I had initially planned to cover them in one article but when I started, it became apparent that there was too much for one article and it should probably be split into each key area in it’s own article.

The first area (topic for this article) is **Tracing / Logging for Maximum Effect**

Let’s kick right off with an example – consider the following trace file output :

10/10/2006 12:34:56.987 LDAP lookup failed with result = -214756544

Yes it tells you exactly what went wrong. A support engineer reading that can clearly see there is a problem with LDAP but nothing else. So what’s his next step ? –  escalate the case to the development team.

Compare that to :

10/10/2006 12:34:56.987 LDAP lookup (‘ CN=Ken Hughes,DC=c2c,DC=com’) to uk.c2c.com failed result = -214756544

A support engineer reading that line can now check :

-   The server uk.c2c.com is up and working
-   Using LDP run the same query and check the results

Maybe the server was down, maybe the query works if the user is logged as an Administrator but not as the account running the software – there is a whole avenue of additional troubleshooting that the support engineer can investigate simply because the trace file was a little more verbose…

 This is true many different areas :-

-   Web Services – don’t just say it failed, tell the user what URL was being used – support guy can check out the web server and DNS resolution
-   SQL stuff – it’s probably apparent which server your going to, but what about tracing out the SPROC your using or the SQL query string itself (especially if it’s dynamically generated in code)
-   Remoting / DCOM – Why settle for CoCreateInstance failed when you could specify the destination server and the class your trying to create

We could all think of more, but the general idea is – make the tracing sufficiently verbose as to allow engineers to investigate why the error occurred. If your using external or third party services (DNS lookups, LDAP, SQL, Exchange, WebServices, DCOM etc etc) then give the details of where your going to and the parameters your using.

Bear in mind that as a developer, if your tracing out magic numbers or sparse data that only really make sense to *you*, then when someone sees it in a trace file *it’s coming straight back to you*…. 

[writing experience essay](http://essayyoda.com/write-my-essay/)