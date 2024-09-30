---
title: "Immersing myself in PowerShell"
date: "2007-02-26T17:57:22"
tags: [
  "powershell",
  "scripting"
]
---
[PowerShell](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/default.mspx) has been around for some time now, what with betas and CTPs. For (the released version of) Vista it became available a month or so ago.

It’s been on my ‘must get to grips with’ list for a while now and I’ve kinda been following some blogs about it, slowly getting a little knowledge here and there.

[Mr Hanselman](http://computerzen.com/) (the oracle for all things technical) has done a couple of podcasts on it and I listened a couple of weeks ago to an episode of [Hanselminutes](http://www.hanselminutes.com/) where he interviewed Bruce Payette. Bruce is the language architect for [PowerShell](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/default.mspx) and has just (2 weeks ago) released a book on it ([Windows PowerShell in Action](http://www.amazon.com/Windows-Powershell-Action-Bruce-Payette/dp/1932394907/sr=8-1/qid=1172510993/ref=sr_1_1/104-5358081-9283901?ie=UTF8&s=books)).

Bought the book last week in the US and have had it open ever since. Everything I do, I now do with a rosy PowerShell perspective. The only way (I find) to really to get to grips with something is to completely immerse yourself in it – think in it, live it, breathe it….

Today I have been updating the server side file for our [Archive One](http://www.c2c.com/) (email archiving for Exchange) auto update feature (we’re just released V5.0 SR1, so the build numbers that are checked have changed). It’s a simple XML file that is parsed for ‘GA’ release version (ProdVer) and ‘HF’ version (hotfix). It looks like this (sample only):

<?xml version="1.0"?>
<Versions\>
    <AOnePolService ProdVer\="5.0.0.1643" HotFix\="5.0.0.1643"\></AOnePolService\>
    <AOneCmplService ProdVer\="4.3.0.1077" Hotfix\="4.3.0.1094"\></AOneCmplService\>
</Versions\>

I wanted to provide a quick and easy way to find the latest version of each products. I came up with the following [PowerShell](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/default.mspx) one liner (split over three lines for readability):

**(\[XML\] (new-object ("net.webclient")).Downloadstring(
"http://support.c2c.com/versioncheck/currentversions.xml")).versions.get\_ChildNodes() |
% { "" } { $\_.psbase.Name + "\`t GA=" + $\_.prodver + "\`t HF=" + $\_.hotfix } { "" }**

Downloads the xml file, parses it and lists out the product, GA version and HF version

![](f14faf75f2814977904c840eb9af2d3dsnip.bmp)

Watch this space for some Active Directory related stuff as I have been very active in scripting AD over the past few days and am in the process of porting it to [PowerShell](http://www.microsoft.com/windowsserver2003/technologies/management/powershell/default.mspx).