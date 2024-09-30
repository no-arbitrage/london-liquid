---
title: "More Hassle With SupportSuite"
date: "2006-11-17T18:19:16"
tags: [
  "technical"
]
---
At [work](http://www.c2c.com), we use [SupportSuite](http://www.kayako.com/supportsuite.php) for support case tracking. This is a real neat product but the support is dreadful.

I was running version 3.0.0.80 and there are a few problems with it, they recently announced version 3.1.40. I set up a testing copy and tried an upgrade – no joy, 404 page after about 0.00065353 nanoseconds, spent ages trying to resolve and / or get an answer out of [Kayako](http://www.kayako.com/) but nothing.

I did like the look of the new version and it solved a number of small but annoying issues we were seeing, so I implemented it in parallel (on port 81) to our current system, we had it all configured, ported across all the Knowledgebase data (Import / Export from the app), ported across all the user accounts (manual DB table hacking) and basically got everything ready for the ‘big bang’ migration.

Anyway I decided the big bang migration should happen this morning (Friday is typically a quiet day) – I had everyone port their own cases across, put links from the old case to the new case and links from the new case to the old case.

Next step was to move the old system to port 8080 and the new system from port 81 to port 80. Here’s what happened :-

-   Changed the website ports (in IIS) so that the old system uses port 8080 and the new system uses port 80
-   Modified the settings (stored in the mySQL database) to ensure the URL was correct (i.e. removed :81 from the end of the URL)
-   Cleared the SupportSuite cache
-   IISRESET
-   Closed all the browser sessions.
-   Opened a browser and browsed to the new site – horrible page with no images and bad formatting.
-   Ctrl F5 to refresh the browser again – same detail
-   Delete all the temporary files from IE followed by another Ctrl F5 – same detail
-   Examine the properties of the images that were not loading – they are still pointing to port 81
-   Ramp up my sense of urgency as we have now been off air for about 30 minutes.
-   Browse the mySQL database and spot a settingcache table – run a SELECT \* FROM settingscache WHERE data LIKE ‘%:81%’
-   AH !! find the row with the cached URL – change the URL and UPDATE, on to a winner here…
-   Ctrl F5 – gets a message stating that SupportSuite is not found and I should run setup again – !!WHAT!!
-   Look at the settingscache table again – AH !! there is a ‘hash’ field – Stumped !!
-   Change the new system back to port 81
-   Added a new website on port 80 with a permanent redirect to port 81…
-   Opened port 81 through our firewall…
-   Fixed !! At last !!!

It seems the settingscache table is built the very first time the web app runs and then is NEVER updated EVER.

It’s working now but I hate leaving niggly issues like this (needing both port 80 and 81 to function correctly) – further investigation on Monday, but if this is a real bug then given my experience with Kayako to date I expect the easiest route will be to configure it on another server (ON PORT 80 !!) and then move it across…