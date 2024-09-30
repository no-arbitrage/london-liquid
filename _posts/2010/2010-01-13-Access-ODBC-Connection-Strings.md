---
title: "Access ODBC Connection Strings"
date: "2010-01-13T22:23:22"
tags: [
  "development",
  "scripting",
  "web"
]
---
I was working on an old (classic) ASP page the other day. It was pulling data from an Access database file and using an ODBC driver to get the connection.

It was working fine on a Windows 2003 server, but when I pulled the file into a local website on my Windows 7 machine (with Office 2010 beta) it kept failing at the ODBC layer. The reported error message was :

> **Microsoft OLE DB Provider for ODBC Drivers error ‘80004005’  
> \[Microsoft\]\[ODBC Driver Manager\] Data source name not found and no default driver specified**

Looks like the driver specified in my connection string couldn’t be found. I was using the following :

    objConn.Open "DRIVER={Microsoft Access Driver (\*.mdb)}; DBQ=c:inetpubwwwrootpstdiscovery.mdb;"

This all looked correct and checking the excellent “[ConnectionStrings.com](http://www.connectionstring.com/access#p88)” website they were saying the same thing – strange. It then struck me that I’m using Win 7 and Office 2010, either of which could have changed the ODBC driver or installed a new driver, so checking the “Data Sources (ODBC)” tool I see that the driver also works with .accdb files, so I’m guessing this is an updated driver.

Changing the connection string (adding the \*.accdb) was the next step.

> objConn.Open "DRIVER={Microsoft Access Driver (\*.mdb, \*.accdb)}; DBQ=c:inetpubwwwrootpstdiscovery.mdb;"

Testing with this new connection string worked fine – problem solved….

[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/AccessODBCConnectionStrings_1365B/image_2.png)

GEO51.4043502807617:\-1.28752994537354