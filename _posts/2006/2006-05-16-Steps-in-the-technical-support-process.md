---
title: "Steps in the technical support process"
date: "2006-05-16T11:05:47"
tags: [
  "technical"
]
---
I have been looking at how to make improvements (in productivity) in our technical support team recently.  
One of the ‘tick list’ items I always put on job Descriptions (when advertising) is ***“Must have a logical and methodical approach to troubleshooting technical problems”***. I went right back to basics and thought about ‘how do I define / measure this and what, in fact, is a logical and methodical approach ?’

Heres what I came up with (at a high level) :

**Step 1 – Determine who is reporting the problem.**  
This is gathering the contact information (email, phone, name, company, preferred method of contact etc) of the person reporting the problem to you. It has to be the first step – if the call drops mid way then you need to know who to call back, when troubleshooting you need to know who to communicate the troubleshooting steps / workarounds / resolutions to.

**Step 2 – Determine the nature of the problem.**  
This is gathering the context information – what product is it, what they are trying to achieve, what they are seeing that they believe is incorrect functionality and what they are expecting to see (the users understanding of the problem).

**Step 3 – Gather the supporting evidence.**  
This is where you get log files, screenshots etc. – i.e. the supporting evidence that allows you to troubleshoot in more detail / see exactly what the product was doing at the time of failure. Usually, the best way to do this is to enable whatever logging your software / system has and have the user / customer carry out the steps which result in the (perceived) failure. If it’s reproducable then that is half the battle… I also suggest that you ask for screenshots as they are good solid evidence and nothing is lost in the communication “Oh, I get an error message about DCOM or something”, or “The screen goes a funny color” – much better to have the a screenshot of the actual message / action that is happening. Of course the problem may be so obvious that you can already reproduce it without the need for support evidence – if so, then great, no need to bother the customer to get the supporting evidence, gather it yourself (ALL your support people have immediate access to a demo/lab system – right !!).

**Step 4 – Outline the next actions.**  
*TELL THE USER / CUSTOMER WHAT TO EXPECT NEXT*. If you can commit to timescales, even better !! – but at a minimum let them know what the next steps are “As soon as I get the trace file from you, I will review it and get back to you, it is likely to be this afternoon sometime, what is the best number to get you on this afternoon”.

This is at a very high level, obviously in a real world scenario the above steps are all interspersed with process steps related to your own systems / infrastructure / policies etc (for example, checking the customer has a valid maintenance contract, opening a ticket in the fault tracking application)