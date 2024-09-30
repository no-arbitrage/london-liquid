---
title: "Outlook IMAP"
date: "2008-06-25T12:24:06"
tags: [
  "web"
]
---
[![OutlookIMAPFolders](OutlookIMAPFolders_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/OutlookIMAP_BCB6/OutlookIMAPFolders.png)Now that I have moved us over to Google Apps For Your Domain (GAFYD) completely, I have been fighting with Outlook to get a decent user / email experience.

It just doesn’t seem to flow well – I got the SMTP/IMAP send/receive stuff all set up and working correctly, but it’s clunky.

I have all the folders appearing in my Outlook client but the UX just does not flow – in order to LABEL a email (in google terms) I have to move it to a folder named as the LABEL I want to apply (if I want to label it ‘Outdoor’ I simply move it to the ‘Outdoor’ folder). Equally, anything I have labelled will be visible in that folder (if I labelled it ‘Outdoor’ then it’ll be in my ‘Outdoor’ folder).

There are plenty of sites with step by step guides for configuring Outlook with GoogleMail, so I wont go into that but I will share this tip:

To download full emails (not just headers) you need to do the following :-

-   **Tools** -> **Send/Receive** -> **Send/Receive Settings** -> **Define Send/Receive Groups…**
-   Select the group your IMAP account is in (typically ‘All Accounts’)
-   Click ‘Edit’
-   Select the account if there is more than one
-   Select the option for ‘Download complete items’ (this is different for OL2003 and OL2007, below is OL2003)

[![OutlookIMAPDownload](OutlookIMAPDownload_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/OutlookIMAP_BCB6/OutlookIMAPDownload.png)

However the biggest pain with Outlook is that it does not by default open in any account that does not have the usual default folders (Contacts, Calendar, Inbox etc).

As a Google IMAP account does not have these it automatically adds a ‘Personal Folders’ PST file and opens at the Inbox folder folder of that instead.

I am pretty much settled on letting OL grab a copy of my mail (just in case I need to edit offline), but am finding myself using Google’s web mail interface most of the time.

GEO 51.4043197631836:\-1.28760504722595