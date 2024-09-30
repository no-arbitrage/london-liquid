---
title: "Multiple Machine Configuration"
date: "2013-03-07T21:50:20"
tags: [
  "productivity",
  "technical",
  "tools"
]
---
If you are anything like me you’ll find yourself using a number of different PCs through a typical week. I have my work laptop, my work Dev machine, my home PC, my netbook and some semi permanent virtual machines that I have. It can be a bit of a pain keeping my standard installed apps updated across all of these.

I’m getting too used to my Android phone and Tablet auto updating their apps with little or no interaction – what I wanted was the same for more of the apps I use on the various PCs I use.  
I haven’t quite got a full solution for it, but I do have a big step toward it – [Portable Apps](http://portableapps.com).[![image](image_thumb.png "image")](https://kapie.com/wp-content/uploads/2013/03/image.png)

[Portable Apps](http://portableapps.com) are pretty good at keeping themselves updated, but the pain is the configuration. For example when I add a command to [Executor](http://executor.dk/) (my launcher of choice) I had to update it on all machines, same with adding a new site to the [Filezilla](http://filezilla-project.org/) Site manager or Putty – it needed to be updated across all machines.  
I could of course store the Portable Apps on a memory stick and carry that everywhere – but then the challenge is – carrying it everywhere.

So I have a solution that brings the ease of Portable Apps with the omnipresence capabilities of my free 25GB SkyDrive account (but you could use any file sync/share application).

[![image](image_thumb1.png "image")](https://kapie.com/wp-content/uploads/2013/03/image1.png)I have a folder in [SkyDrive](https://skydrive.live.com/) that I have installed my chosen Portable Apps to, so they turn up on every machine.  
I also have an install folder with tools and scripts that I run on each new machine that I install SkyDrive on that gives me common locations for the apps (regardless of the user I am logged in as), updates the PATH variable, adds apps to the Start folder and the like

The script does a number of things :-

It creates symbolically linked folders so I can go to c:\\apps instead of c:\\users\\kenh\\skydrive\\apps (or different usernames on each machine)

> rmdir c:\\tools  
> rmdir c:\\apps  
> rmdir c:\\scripts  
> mklink /d c:\\Tools "%userprofile%\\SkyDrive\\Tools"  
> mklink /d c:\\Scripts "%userprofile%\\SkyDrive\\Scripts"  
> mklink /d "c:\\Apps" "%userprofile%\\SkyDrive\\Apps"

It adds some data to the registry

> regedit.exe /f CommandPromptHere.reg

It uses a tool called [xxmklink](http://www.xxcopy.com/xxcopy38.htm) (from the makers of xxcopy) to add a shortcut to Executor to the start up folder so that it runs every time Windows starts.

> xxmklink "C:\\ProgramData\\Microsoft\\Windows\\Start Menu\\Programs\\Startup\\Executor.lnk" "C:\\Apps\\PortableApps\\Executor\\Executor.exe" "" "C:\\Apps\\PortableApps\\Executor"

It starts some applications

> START "C:\\Apps\\PortableApps\\Executor\\Executor.exe"  
> START "C:\\Apps\\Start.exe"

And the final thing it does (currently) is to update the PATH variable using pathman.exe

> pathman.exe /as c:\\tools

This makes my life much easier, I can be the same kind of productive regardless of the machine I am working on – they all have the same configuration, versions of software and paths, and the best bit is that when Portable Apps updates itself on one machine, it is reflected on all others within a few minutes.

I need to look at how I can extend this further, with introducing more portable apps to replace heavyweight desktop installs. I’ll keep you updated.