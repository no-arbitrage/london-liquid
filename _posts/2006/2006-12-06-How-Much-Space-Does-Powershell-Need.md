---
title: "How Much Space Does Powershell Need"
date: "2006-12-06T02:36:30"
tags: [
  "technical"
]
---
OK I know that Powershell V1.0 is not supported on Vista (yet) and that Powershell RC2 is only supported for Vista RC1 – at least [that’s what it looks like from the Powershell Website.](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/download.mspx)

I thought I’d give installing Powershell R2 a bash just to see how far I’d get with it. Here’s how it went :-

[![](PowershellProperties_thumb1.png)](https://kapie.com/content/binary/WindowsLiveWriter/HowMuchSpaceDoesPowershellNeed_16B1/PowershellProperties3.png) Powershell R2 is a 1.6MB file as we can see from the properties status bar thingy in Vista.

Should need too much disk space right – even with compression it couldn’t want more than say 160MB ??

OK, lets kick off the install

 [![](PowershellInstall_thumb3.png)](https://kapie.com/content/binary/WindowsLiveWriter/HowMuchSpaceDoesPowershellNeed_16B1/PowershellInstall5.png)

Good – it started, and looks to be extracting files etc.

[![](PowershellNotEnoughSpace_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HowMuchSpaceDoesPowershellNeed_16B1/PowershellNotEnoughSpace2.png)

Oh – oh – not enough space eh? Maybe one of my drives is getting low ?

[![](FreeDriveSpace_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HowMuchSpaceDoesPowershellNeed_16B1/FreeDriveSpace2.png)

Doesn’t look like it, 15GB free on one and 25GB on the other – looks like the error message is a tad misleading. Anyway as expected the result was…

[![](PowershellInstallFailed_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/HowMuchSpaceDoesPowershellNeed_16B1/PowershellInstallFailed2.png)

I guess I shouldn’t complain, it is a BETA and it states on their Website that it wont work. However maybe a version check before starting the install or a more accurate / useful error message would be better ?…