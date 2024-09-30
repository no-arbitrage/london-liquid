---
title: "Writing Supportable Software 3 – Identify your dependencies"
date: "2006-10-30T09:22:55"
tags: [
  "development"
]
---
This is the third article on Writing Supportable Software, helping all those customers and support engineers work with our software products and troubleshoot them themselves (i.e. going as far as possible down the troubleshoot path before referring it back to you).

[You can the first article here…](https://kapie.com/Writing+Supportable+Software+1+Tracing+For+Maximum+Effect.aspx)  
[and the second article here…](https://kapie.com/2006/10/28/Writing+Supportable+Software+2+How+Easy+Is+It+To+Roll+Back.aspx)

This article is all about identifying when and how you are using external systems and services. Most enterprise application work with external technologies or services, for example the company I work for (C2C) build applications that work with Microsoft Exchange Server, maybe you consume a particular web service or make LDAP calls.  
Whilst you can reasonably expect the customer to provide a stable working environment for your system / software, life is not always that rosy. I’ve worked with Enterprise systems that have been dependant on SQL Server, Exchange, LDAP, Web Services, VoIP gateways and many other systems…

For experience, there is nothing that irks a customer more than a vendor who points the finger *blindly* at some part of the environment or system infrastructure. As a support guy for a vendor you have to take the pain and go that extra mile to hold the customers hand and demonstrate how / why / where the environment is going wrong.  
Developers can make this considerably easier by identifying (in a log file or the like) exactly what services your calling out to – and how long it’s taking.

I can’t tell you how many times I’ve been troubleshooting an issue only to eventually end up some service the software was expecting to just work was not just working – but that’s only half the battle, when you’ve found that you then need to demonstrate to the customer that it’s the service that is not working (and if it was working then your system would be working also)

A great example of this is customers have some kind of screwed up AD implementation, so we’ll make a LDAP call thinking it’s going to the local DC but in reality it finding some old dusty DC in Alaska on the end of a Dial Up connection and it will time out. Another would be a system making calls to a SQL server but the machine making the calls cannot connect to the SQL Server, maybe because there is a firewall inbetween (and the SQL network port 1433 ?? is locked down)

So, when writing your code, identify these external service and provide the verbosity in your log files or your application status screen that allow the support engineers to determine which services are failing (and to demonstrate the fact to the customer)

I haven’t even touched on how often we find that our systems / software stops working, customer calls and you spend hours troubleshooting only to find that the customer took a service offline, or decommissioned a server, or added a firewall – you get the idea…