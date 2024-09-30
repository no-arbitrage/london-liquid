---
title: "Dasblog – Daily Activity Report"
date: "2006-11-08T15:02:13"
tags: [
  "dasblog"
]
---
One of my gripes with [dasBlog](http://www.dasblog.net) is the reporting. To get to reports I have to browse to my blog, login and then view the reports online – bit of a pain doing this every day (yes I am so vain that I like to see my stats on a daily basis).

So I spent a few hours recently adding an ‘Daily Activity Report’ feature.  It was surprising easy – checkout (get a working copy) of the source from [sourceforge](http://sourceforge.net/) (browse it here [https://svn.sourceforge.net:443/svnroot/dasblogce/trunk](https://svn.sourceforge.net:443/svnroot/dasblogce/trunk "https://svn.sourceforge.net:443/svnroot/dasblogce/trunk")), familiarize yourself with it, add your changes and create a patch. Send the patch to someone with admin rights (so it can be checked in) and that’s it… I’d recommend to anyone to try it out…

Anyway the ‘Daily Activity Report’ feature – basically starts a background thread which caches the current date and then ticks every hour. Each tick it will check the current date against the cached date and if they do not match then it will send an email to either the Notification list or the Site Owner / Administrator. It is configured by simply checking a box in the configuration page (in the SMTP Server section)

[![](Capture_11-08-2006_2_thumb%5B1%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/DasblogActivityReports_CF46/Capture_11-08-2006_2%5B1%5D.png)

The email contains similar detail to that of the ‘Activity… Referrers’ page.

[![](Capture_11-08-2006_1_thumb%5B1%5D.png)](https://kapie.com/content/binary/WindowsLiveWriter/DasblogActivityReports_CF46/Capture_11-08-2006_1%5B1%5D.png)

I also made the method / class that implements the creating / sending the report public so that it can be extended – I was thinking along the lines of a ‘Send Daily Activity Report Now’ button or enhancing the Mail-To-Weblog to monitor for a specific phrase in the subject and if found to send the report (maybe also specify the date)…

Watch this space for updates…