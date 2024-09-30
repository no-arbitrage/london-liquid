---
title: "Virtual Server Annoyances"
date: "2006-10-25T12:48:13"
tags: [
  "technical"
]
---
The company I work for ([C2C Systems](http://www.c2c.com/)) are a [Microsoft Gold Partner](https://partner.microsoft.com/UK/program/programoverview/goldcertpartner) this is great in that it gives us a whole bunch of licenses for internal use, it also gives us (almost ??) unlimited access to software for test, development and demonstration. The problem is :-

*Demonstration licences are provided for customer demonstration purposes only – that is, licences to be used only by employees of the company with customer contacts.*   
\[extracted from the Microsoft Partner Programme document (v7.1)\]

When you think about it, this is incredibly limiting – it means that someone from the company has to be present at the demonstration (in person or remotely)…

Now, our solution (Archive One, email archiving) works with Exchange Server, our normal demos are on a system with Exchange 2003 (*aside: although we do work with Exchange 2007 already*) meaning we need Windows 2003 and all the Active Directory baggage.

Virtual Server technology (in fact any virtualization technology) allows me to generate a number of Virtual Machine images and run them on one machine and hey presto I have a full demo / evaluation environment – the limiting factor is licensing – If I want to just give people (resellers and / or customers) copies of those images so that they can easily test / evaluate our solution then I have to license it (Win2003, Exchange 2003, WinXP etc)

I think Microsoft really missed a trick here – some kind of (slightly) crippled or trial version of the OS and apps, *that partners can redistribute*, would have been great – **BANG** – you have a easy way for everyone to roll out full demo environments of there software / systems.

As a customer I can grab the DVD (or download the images) and evaluate without having to go through the pain of putting together a bunch of servers, setting up a test environment yadda yadda…

As a partner I can grab the DVD (or download the images) and immediately go out and start demo’ing it to my customers (the same way that vendor does)…

Anyway, I’m trying to beat up the Virtual Server / Licensing team over this…