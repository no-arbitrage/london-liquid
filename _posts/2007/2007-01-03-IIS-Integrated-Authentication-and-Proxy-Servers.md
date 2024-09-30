---
title: "IIS Integrated Authentication and Proxy Servers"
date: "2007-01-03T22:32:46"
tags: [
  "technical"
]
---
We had a customer issue yesterday that manifested itself as the user getting a HTTP 401 error when trying to connect to a Website (that is part of our Product).

The user was logged into the domain, the virtual directory was set for ‘Windows Integrated Authentication’ so they should have been able to connect no problem.

After so investigation we opened the IIS logs and found that the substatus code was 2 (HTTP Error 401.2 – Access denied by server configuration). A bit of searching around the Microsoft KB unearthed this article:

[Troubleshooting HTTP 401 errors in IIS](http://support.microsoft.com/kb/907273)

**Common reasons**

• No authentication protocol (including anonymous) is selected in IIS. At least one authentication type must be selected. For more information, click the following article number to view the article in the Microsoft Knowledge Base:

> [253667](http://support.microsoft.com/kb/253667/) (http://support.microsoft.com/kb/253667/) Error message: HTTP 401.2 – Unauthorized: Logon failed due to server configuration with no authentication

• Only Integrated authentication is enabled, and an older, non-Internet Explorer client browser tries to access the site. This happens because the client browser cannot perform Integrated authentication. To resolve this problem, use one of the following methods:

> • Configure IIS to accept Basic authentication. This should only occur over SSL for security purposes.

> • Use a client browser that can perform Integrated authentication. Internet Explorer and new versions of Netscape Navigator and Mozilla Firefox can perform Integrated authentication.

• Integrated authentication is through a proxy. This happens because the proxy doesn’t maintain the NTLM-authenticated connection and thus sends an anonymous request from the client to the server. Options to resolve this problem are as follows:

> • Configure IIS to accept Basic authentication. This should only occur over SSL for security purposes.

> • Don’t use a proxy.

*Turns out our customer was using a Proxy Server – adding the FQDN of our Website component to the proxy exclusions list solved the problem.*