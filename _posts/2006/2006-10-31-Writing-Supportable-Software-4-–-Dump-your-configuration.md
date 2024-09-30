---
title: "Writing Supportable Software 4 – Dump your configuration"
date: "2006-10-31T12:17:38"
tags: [
  "development"
]
---
This is the forth (and last for the moment) article on Writing Supportable Software, helping all those customers and support engineers work with our software products and troubleshoot them themselves (i.e. going as far as possible down the troubleshoot path before referring it back to you).

[You can the first article here…](https://kapie.com/Writing+Supportable+Software+1+Tracing+For+Maximum+Effect.aspx)  
[and the second article here…](https://kapie.com/2006/10/28/Writing+Supportable+Software+2+How+Easy+Is+It+To+Roll+Back.aspx)  
[and the third article here…](https://kapie.com/2006/10/30/Writing+Supportable+Software+3+Identify+Your+Dependencies.aspx)

This final article is all about paranoia and the fact that customers (as much as we love them, and need them to pay our salaries) are not to be trusted *one little bit*…

We’ve all been there, troubleshooting a problem, ask the customer what parameter X is set to, go off down a whole path of investigation, find nothing, scratch our head, check parameter X for ourselves and find it’s set to something different (and the whole problem resolution falls into place).

It’s not that customer purposely try to be difficult, simply that they don’t know the system / software as well as you do – quickfire questions about the ***Primary Master Lookup Port*** and the ***Lookup Master Access Port*** get them all confused – they’re probably under pressure for their boss to get it fixed, they’ve tried a whole load of things, the last time they looked it was set to *5465ABBG\_Master* but in the confusion they forgot they changed it to *Lets\_Just\_Try\_This\_Option*.

They best way to determine what variable / options / configuration your system is using, is to write it out to a trace file, on start up, before a processing run, before you use it (*whenever is appropriate* – *and that means, if it could have been changed between now and last time we wrote it to the log then write it again*)

If ever there has been a thing that wastes the time of support people it is working on the ***assumption*** that, the value the customer told them it was set to was the actual value that it was set to.