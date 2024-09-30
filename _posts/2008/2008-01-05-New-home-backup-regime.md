---
title: "New home backup regime"
date: "2008-01-05T00:43:15"
tags: [
  "scripting",
  "technical"
]
---
***The king is dead, long live the king.***

Over the Christmas break the license for my beta of [Windows Home Server](http://www.microsoft.com/windows/products/winfamily/windowshomeserver/default.mspx) ran out, so I needed an alternative backup / storage solution. I briefly considered Linux with some iSCSI software, Windows with DFS or FRS, or indeed forking out some of my scheckles for a folder sync application.[![quicklinks_whs_logo](quicklinks_whs_logo_3.gif)](http://www.microsoft.com/windows/products/winfamily/windowshomeserver/default.mspx)

The requirements were as follows:-

-   NTFS, for large file support (12 Gb in some cases).
-   Easy duplication of the data (including hierarchy) across multiple drives.
-   UNC pathname support, so I could ‘rehome’ my docs, music, photos etc to it.[![ide_newlogo](ide_newlogo_3.jpg)](http://www.idrive.com/)

In the end I opted for a fairly simple solution :-

-   A windows machine with a drive for the OS and two additional data drives.
-   One of the additional drives would be the primary where folders are ‘rehomed’ to and all data is stored.
-   A batch file would fire off ‘Robocopy’ (free in the [Windows Resource Kit](http://www.microsoft.com/downloads/details.aspx?displaylang=en&familyid=9d467a69-57ff-4ae7-96ee-b18c4790cffd)) to mirror this primary data drive to the secondary data drive.
-   Another batch file would fire off ‘Robocopy’ for copying to external USB drives.
-   Batch files would be scheduled using AT command line tool and would email results files using the free Blat! command line tool.[![flickr_logo_gamma_gif_v1_5](flickr_logo_gamma_gif_v1_5_3.gif)](http://www.flickr.com/)
-   The primary data drive would also be backed up to my [‘iDrive Pro’](http://www.idrive.com/online-backup-features.htm) account (online 150 Gb storage facility for $50 / year).
-   Of course, photos are also backed up to my [Flickr Pro](http://www.flickr.com/gift/) account (unlimited online storage of images for $25 / year).

GEO 51.4043197631836:\-1.28760504722595