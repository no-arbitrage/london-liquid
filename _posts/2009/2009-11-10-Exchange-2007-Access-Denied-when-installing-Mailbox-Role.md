---
title: "Exchange 2007 Access Denied when installing Mailbox Role"
date: "2009-11-10T06:43:12"
tags: [
  "technical"
]
---
Yesterday as I was trying to install Exchange 2007 on Windows 2008 R2 Enterprise Edition, I got a pretty strange issue.

[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/Exchange2007AccessDeniedwheninstallingMa_5DA7/image_2.png)Everything worked well up till the ‘installing mailbox role’ phase – it seemed to fail pretty quickly when it got there. Subsequent retries also failed (quickly) with exactly the same error – ‘Access Denied’

A little bit of Googling uncovered that the ‘Setup’ for Exchange 2007 had to be run in ‘Compatibility mode’ for it to work correctly. Crazy I know, but it works.

Open a Windows Explorer to the DVD, right click on the setup.exe file and choose the Compatibility tab, then set it to Vista Service Pack 2 – now run it, and it should all work as expected.

GEO51.4043502807617:\-1.28752994537354