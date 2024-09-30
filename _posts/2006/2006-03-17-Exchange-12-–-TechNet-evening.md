---
title: "Exchange 12 – TechNet evening"
date: "2006-03-17T10:07:59"
tags: [
  "technical"
]
---
Mr Burford and I went to a TechNet evening at Microsoft last night. The topic was ‘Exchange 12 Overview’

It proved to be very interesting, lots of good info. Below are some (sketchy) notes I made…

-   Release date is likely to be late 06 / early 07.
-   The installer will run Best Practices Analyser (ExBPA) by default – if you don’t meet the prereq’s the install will terminate.
-   You cannot install EX12 into a domain / forest that has EX5.5 on it.
-   Exchange 12 CTP is likely to be available on the March 06 TechNet and MSDN packages.
-   Outlook 12 will have a ‘autoconfigure’ feature where you can simply enter your email address and password and it will find the server your mailbox is hosted on and do all the necessary configuration (Outlook profiles will not be needed – not clear if they are dissappearing or staying as an option). This autoconfigure is going to based / served from DNS. It may provide hooks for 3rd parties to load / configure their addins.
-   ESM will use MMC version 3.1 (the same as MOM currently uses). This provide a treeview on the left, the content in the middle and then access to specific tasks / wizards on the right
-   There will be ‘Policy Rules’ on the back end that all messages will pass through. This is a sort of ethical firewall – Bob is not allowed to send emails to Jane unless it has the word Emergency or the word Fire in the subject. This is along the lines of Inbox rules but everyone’s email will pass through it.
-   There will be a increase in the limit of Storages groups and Databases per group. 50 storage groups with up to 50 databases in each. Prefferred architecture is 1 DB per SG
-   Disk and Memory are the current bottlenecks in EX2003. MS have seen this internally with 4K users per server, users that are a member of many Security groups put a much increased load on the server (ACLs for each user are cached in memory – lots of groups = more memory required to cache)
-   Cached mode puts an increased load on the server as it open multiple MAPI connects per user concurrently.
-   As they are using 64bit only this increases the memory addressable to 4 exabytes (??), but they reckon the ‘sweetspot’ for EX12 will be 32GB of memory.
-   The will be increasing the amount of data cached from XX (something fairly small) to 5MB per user. The disk IO should also improve as it means the same data can be loaded in half the IO ops. They are also doubling the page size to 8K (for increased performance)
-   OWA architecure is changing – these are now called Client Access Servers (CAS), they provide all access to mailboxes except MAPI (which can speak directly to a Mailbox Server). The CAS will generate all the HTTP / HTTPS etc that the user sees but it will speak to the users mailbox via MAPI.
-   OWA is using AJAX (a technology that allows you to update webpages without having to submit the whole page, get the blank screen and wait for the refresh to happen – our new helpdesk app uses this also)
-   All the MAPI client bits will be removed from the Mailbox Server service, so you will be able to install OL on a MB server and it is a supported environment (no MAPI conflicts etc)
-   You will be able to access Sharepoint shared documents areas through OWA
-   They will be able to define (virtual) folders in users mailboxes that have different retention policies. For example you will be able to set the retention policy for peoples Inboxes as 90 days and tell them anything that is legal related then move it into the legal folder where it will get a retention policy of 7 years. BUT – it relies on the user moving it into the folder. You can have multiple of these folders each with different retention periods.
-   Full Text Search is switched ON by default, you will also be able to conduct multiple mailbox searches.

The other area was unified messaging (UM). I come from a VoIP / Multimedia call centres / telecoms background that were to some extent dealing with UM / intelligent voicemail platform. With EX12 this technology comes of age and becomes mainstream. They showed us a live working demo last night of someone calling into a VoIP gateway / PBX and accessing EX12. Here was the flow :

-   EX12 : Please enter your account number and password
-   User : \[enters numbers\]
-   EX12 : Hello \[firstname\], tell me what you would like to do. For appointments say ‘Calendar’… etc
-   User : Calendar
-   EX12 : Accessing your calendar, which day would you like
-   User : Tuesday
-   EX12 : First appointment for Tuesday 21st March is \[appointment details\]
-   user : I will be 10 minutes late
-   EX12 : \[sends updates to all meeting attendees telling them you will be 10 minutes late\]
-   User : Clear my calendar
-   EX12 : Clearing your calendar, how long for
-   User : 2 hours \[or 2 days\]
-   EX12 Would you like to recod a message to all the meeting attendees
-   User : \[records a message\]
-   EX12 : \[Cancels all the meetings scheduled within the period by sending out cancellations with the recorded message attached\]

It will also read text, emails etc to you. When someone leaves you a VM it arrives in your mailbox as a messageclass of ‘Voicemail’ (basically a message with a WAV or WMA file attached). You can listen to the audio (and jot down notes into the VM message object as your listening – e.g. writing down a phone number). If your viewing the message on your PDA or Phone via active sync or OWA you can have EX12 telephone your mobile and play the audio stream to you. Very cool stuff