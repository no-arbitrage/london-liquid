---
title: "MAPI and Unicode PST files"
date: "2006-11-16T13:12:23"
tags: [
  "development"
]
---
At [work](http://www.c2c.com/) we are heavily into MAPI (we provide email archiving solutions).

MAPI is Microsoft’s mainstream supported and recommended protocol for accessing Exchange. There are currently two main versions of MAPI – ‘Exchange MAPI’ and ‘Outlook MAPI’.

Exchange MAPI is the version that is installed when you install Exchange System Manager (ESM) and is the recommended version to use for enterprise applications that work with Exchange.  
Outlook MAPI is the version that is installed when you install Outlook – it is NOT recommended for enterprise applications.

Now, there are also [two types of PST files](http://support.microsoft.com/kb/830336) – the original Outlook 97-2002 version and the newer Outlook 2003 Unicode version (Outlook 2003 supports both versions, but no other version of Outlook supports the Unicode version).

The problem is that the ESM version of MAPI does not support the 2003 / Unicode version of PST files either. This is real pain, meaning that we have to use the ESM version (if we want to go with Microsoft’s best practices) for our application, but if we want to support the Unicode PSTs we need to somehow use the Outlook version (NOTE: Microsoft absolutely do NOT support both versions of MAPI installed on the same machine).

Apparently the new MAPI 2007 version (that will be downloadable for use with Exchange 2007) was initially slated to support both, but, I’ve since heard this will not be the case. So currently (and possibly in Exchange / Outlook 2007), the only way to work with Unicode PSTs is via Outlook MAPI.

[internet payday loan](http://aupaydayloans.com)