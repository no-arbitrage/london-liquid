---
title: "Homebrew Home Server"
date: "2008-02-13T12:46:49"
tags: [
  "technical",
  "tools"
]
---
So a while back (31st Dec 2007), my beta license for [Windows Home Server](http://www.microsoft.com/windows/products/winfamily/windowshomeserver/default.mspx) (WHS) expired and [I hacked together an alternative](https://kapie.com/2008/01/05/New+Home+Backup+Regime.aspx) [![quicklinks_whs_logo](quicklinks_whs_logo_thumb.gif)](https://kapie.com/content/binary/WindowsLiveWriter/HomebrewHomeServer_B357/quicklinks_whs_logo_2.gif)solution.

I have been updating my (almost) free/opensource alternative (it still needs a Windows OS) over the past couple of days and now have a pretty viable solution.

I have a machine (the Home Server) running Windows (any version would do) with two large additional drives in it (Data1 and Data2).  
Data1 is the primary data drive and on there I created a number of folders / shares:[![HomeServerFolders](HomeServerFolders_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HomebrewHomeServer_B357/HomeServerFolders.png)

-   Photos
-   Documents
-   Music
-   Videos
-   Software
-   Backups
-   Downloads

I re-homed each of my ‘special’ folders in Vista (Docs, Music, Video, Photos) to these shares, so all data is stored on the Home Server. You could create a separate shared folder for each user with the correct permissions, but I share all the docs/photos etc between all machines so no need for me.

Next I wanted the WHS feature of duplicating the stored data across more than drive, so I grabbed a copy of the [Robocopy](http://en.wikipedia.org/wiki/Robocopy) and created a batch file with the following commands :

-   ***robocopy d:documents e:documents /MIR /SEC /LOG:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:music e:music /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:videos e:videos /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:software e:software /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:photos e:photos /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:backups e:backups /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***
-   ***robocopy d:downloads e:downloads /MIR /SEC /LOG+:c:robocopy.txt /NDL /NFL***

This replicates all the folders across to the other data drive (Data2) thereby mitigating against a single drive failure. All the replication results / logs are stored in a file (c:robocopy.txt) and I wanted that emailed to me so I grabbed a copy of Blat and added the following command line to the batch file :[![blat](blat_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HomebrewHomeServer_B357/blat_2.png)

-   ***c:toolsblat262fullblat.exe c:robocopy.txt -to YOUREMAILADDRESS -subject “RoboCopy Results” -server mail.YOURMAILSERVER.com-f “RoboCopy on Home Server” -u YOURUSERNAME -pw YOURPASSWORD***

I named the batch file ‘replicate.bat’, put it in the c:tools folder and then scheduled the batch file to run every night at 2am with this command line :

-   ***SCHTASKS /Create /SC DAILY /TN Replicate /TR c:toolsreplicate.bat /ST 02:00***

Excellent – now the data is replicated across two drives, and I get an email every day with the results of the replication process (in case anything goes wrong).

[![HTTPFS](HTTPFS_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HomebrewHomeServer_B357/HTTPFS.png)Next I wanted to ensure I have remote access to my files from anywhere. I grabbed a copy of the excellent HTTP File System and put that on the Home Server.

[![HFSScreenshot](HFSScreenshot_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HomebrewHomeServer_B357/HFSScreenshot.png)I set the root to the Data1 drive, created a user account for myself and gave it ‘upload’ ability and that gives me fully web based access to upload and/or download any file.

The next piece in the puzzle is to get full backups of the machines. For this I had planned to use [VMware Server](http://www.vmware.com/products/server/) and the excellent [VMware Converter](http://www.vmware.com/products/converter/) tool, however it seems the command line options for the tool they provide p2vtool only works with a licensed version.

It’s a great tool and pulls a whole physical machine image into a [VMware](http://www.vmware.com/) virtual machine – and is a great way to get the failed machine back to life – what it doesn’t do is restore a machine, but I’m most likely to rebuild any failed machine anyway – I simply need access to any files / data on there that might not have made it to the shared server folders…

GEO 51.4043197631836:\-1.28760504722595