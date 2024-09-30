---
title: "Enabling virtualization when booting from VHD"
date: "2009-11-23T09:51:31"
tags: [
  "productivity",
  "technical"
]
---
[![image](image_thumb.png "image")](https://kapie.com/content/binary/WindowsLiveWriter/EnablingvirtualizationwhenbootingfromVHD_86B5/image_2.png)I ran into some trouble with Hyper V the other day – I had booted from a VHD into Windows Server 2008 R2 and was trying to start a VM – I got the usual  ‘The virtual machine could not be started because the hypervisor is not running” error.  
I had just had a BIOS failure on the machine so I figured it may have switched hardware virtualization support off in the BIOS when it reloaded the defaults.

Checking the BIOS, I found it was switched on – strange. I Googled a bit but everything seemed to be around flipping the setting in BIOS, when I knew to be correct.  
Some further investigation around the boot environment and BCDEDIT settings I found the parameter “HypervisorLaunchType”, thinking this could well be connected, I set the parameter to “auto” in the BCD configuration:

BCDEDIT /set {big-long-guid} hypervisorlaunchtype auto 

This fixed it !!  
So now all my BCD configurations go like this:

BCDEDIT /copy {current-or-guid} /d "New Boot Option"
BCDEDIT /set {new-guid} device vhd=\[V:\]vmimage.vhd
BCDEDIT /set {new-guid} osdevice vhd=\[V:\]vmimage.vhd
BCDEDIT /set {new-guid} detecthal on
BCDEDIT /set {new-guid} hypervisorlaunchtype auto

GEO51.4043502807617:\-1.28752994537354