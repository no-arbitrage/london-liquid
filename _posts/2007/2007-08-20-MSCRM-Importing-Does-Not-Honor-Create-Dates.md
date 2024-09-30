---
title: "MSCRM Importing Does Not Honor Create Dates"
date: "2007-08-20T13:07:39"
tags: [
  "development"
]
---
Some of the code I have for importing data (from ACT! 2000) to MS CRM creates new ‘PhoneCall’ activities / objects. The problem is, that it seems MSCRM does not allow you to programmatically modify the ‘create date’.

Here is the code I use…

> phonecall pc

            CrmDateTime start = new CrmDateTime();

            start.Value = DateTime.Parse(“10/08/2005 12:30”);

            pc.actualstart = start;

            pc.scheduledstart = start;

            CrmDateTime end = new CrmDateTime();

            end.Value = DateTime.Parse(“10/08/2005 14:30”);

            pc.actualend = end;

            pc.scheduledend = end;

            pc.subject = “Phone call regarding sales of Widgets Q2/2005”);  
\*\*\*

            string desc =  “Start    : “ + pc.actualstart.Value + “n”;

            desc += “End       : “ + pc.actualstart.Value + “nn”;  
\*\*\*

            desc += “The details of the phone call go in here”);

            pc.description = desc;

            pc.regardingobjectid = new Lookup();

            pc.regardingobjectid.type = EntityName.contact.ToString();

            Guid contactGuid = new Guid(guidOfContactWeTelephoned);

            pc.regardingobjectid.Value = contactGuid;

The ***actualstart*** and ***scheduledstart*** (and ends) get populated with the current datetime (this seems to happen if the time they are set to is in the past).

Note the two lines between the \*\*\*’s – this is my solution / workaround and simply include the start/end times as text in the body/description of the phone call object.