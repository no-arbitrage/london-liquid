---
title: "MSCRM reports not working"
date: "2008-09-25T21:36:15"
tags: [
  "technical"
]
---
[![MSCRM](MSCRM_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/MSCRMreportsnotworking_13DDF/MSCRM.png)A couple of problems I’ve come across at work with implementing [MSCRM](http://www.microsoft.com/uk/dynamics/crm/product/overview.mspx). These could be bugs, or may not be, who knows (I don’t have the time for a detailed analysis…)

I have a [MSCRM](http://www.microsoft.com/uk/dynamics/crm/product/overview.mspx) 4.0 multi tenant implementation – I ‘disabled’ the first (and default) tenant and suddenly all reports (for all tenants started working intermittently – often just hanging time after time). The workaround I found was to re-enable the first tenant.

This same action (disabling the first tenant) also seemed to screw around with the default tenant (the tenant it chooses when you just browse to [http://crmserver/](http://crmserver/))  – just choosing one at random – this is not something I have resolved yet.

Hope this helps someone..

GEO 51.4043197631836:\-1.28760504722595