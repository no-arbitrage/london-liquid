---
title: "Automating Installs"
date: "2007-08-27T20:21:51"
tags: [
  "productivity",
  "scripting",
  "technical",
  "tools"
]
---
I was spending far too much time installing OS’s – virtual machines, lab machines etc.

[![Unattended](Unattended_1.png)](http://unattended.sourceforge.net/step-by-step.php) In order to automate / streamline this I wanted to look at not just the Windows tools as well as other options. Remote Installation Service (RIS) and unattended.txt files go so far, but during my investigations I came across ‘[Unattended](http://unattended.sourceforge.net/)‘. This open source tool takes unattended.txt, mixes in silent installs for hundreds of other common applications and supercharges the whole lot…

So the deal is, you extract some files from 4 zip archives, configure a DNS alias, share the folder, copy over the i386 folder from your OS CD/DVD, burn an ISO (or create a boot disk) image and your done – 25 minutes end to end.

The boot CD/Disk loads some network drivers, maps a Z drive to ‘ntinstallinstall’ (the machine and share with the files and OS on it) and passes control to a bunch of Perl scripts, these ask some questions from which it creates an unattended.txt file and executes the OS install (reboots and all). When the install completes it can also (optionally) run silent installers for other applications (Office, Open Office, Acrobat Reader, PDF Creator, Visual Studio, Perl etc..) as well as Windows Updates and critical fixes (they keep an up-to-date list on the homepage).

So, in summary, after booting from the install CD then 2 minutes of console based questions I can leave things for an hour or two and come back to a fully installed Windows OS, office applications, sales tools, developer tools – whatever. The scripts that install the additional apps are customizable (you can even enter your product keys) and you can build up suites from individual scripts (so I can have a script to install Visual Studio, another for the MSDN library, another for each of the various developer tools and then I can combine them all into a ‘developer\_machine’ script…

Have a look – if you are doing more than one install (even just two) then this can save you time…