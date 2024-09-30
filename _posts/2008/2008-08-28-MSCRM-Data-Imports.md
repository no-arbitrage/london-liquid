---
title: "MSCRM Data Imports"
date: "2008-08-28T19:28:54"
tags: [
  "technical"
]
---
[![MSCRM](MSCRM_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/MSCRMDataImports_1200A/MSCRM.png) I have just completed a project of migrating ACT! 6 (2000) to [MSCRM 4.0](http://www.microsoft.com/uk/dynamics/crm/product/overview.mspx)

It has not been too difficult, I used the [Northwoods Software ExporterPro](http://www.crmaddons.com/p-15-export-pro.aspx) utility to export all the ACT! 6 data to Microsoft Access, upsized this to SQL and then used a lot of SQL statements and the Data Migration Wizard to get all the data into MSCRM in the correct format.

Anyway, on looking at importing ad hoc data  (Marketing campaign responses, new contacts for existing accounts etc) it seems there is a whole area of functionality designed to allow users to do just that :

-   Get the data in CSV format
-   Create a new ‘data import job’
-   Map the fields across to the chosen entity
-   Let it do it’s stuff

This all works fine as long as you are not referencing the data to be imported to an existing MSCRM entity (for example loading in campaign responses that are related to an existing campaign) – for some reason this failed every time with the error :

***The source data was not in the correct format***

After considerable investigation it seems that you can link data to an existing entity but MSCRM has to ‘automatically’ map the fields. for MSCRM to automatically map the fields all the headings in my source file must have exactly the same name as the corresponding field in MSCRM (you are not given the choice of creating a data map it just jumps to the next step and indicates the data map as ‘automatic’.

So I can now successfully import my campaign response data with the Parent Campaign field set to the name of the parent campaign (not the GUID) and it will intelligently link the imported record to the correct campaign. Seems crazy that it does not allow you to do the same with a manual import, but hey ho..

GEO: 51.4043006896973 : \-1.28754603862762