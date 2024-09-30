---
title: "TaHoGen – Code Generation"
date: "2006-09-28T21:49:02"
tags: [
  "development"
]
---
Quick update on the [TaHoGen](http://sourceforge.net/projects/tahogen) open source project…

I just tidied up the whole source tree, added all the projects in a logical layout to one solution. I also sorted out the Visual Studio addin with better error handling and added a Setup project so that it can be easily installed by an end user.

Murad’s excellent SchemaBrowser was added added and checked in, now allowing us to glean data from databases to use in the code generation. We’re closing in on CodeSmith…

Next step is to demonstrate use by including a number of templates.

The problem with the addin (that was so difficult to remedy) was MDA’s – difficult to troubleshoot and even more difficult to fix.

When the addin is called by VS it tries to save a reference to the instance of VS that is creating it – this is so we can get a handle on the OutputWindow and add some new panes to it.

These sometimes (intermittently) get blocked by MDA’s and the panes do not get created – the panes only show build output text so I added some error handling to continue processing even if we cannot write to the pane.

Anyway, if you want to take it for a test run, you can get it [here.](http://sourceforge.net/project/showfiles.php?group_id=149510)