---
title: "Windows 7 and my Windows Mobile phone"
date: "2009-02-26T20:17:23"
tags: [
  "technical"
]
---
[![logo_windows](logo_windows_thumb.gif "logo_windows")](https://kapie.com/content/binary/WindowsLiveWriter/Windows7andWindowsMobilePhone_11776/logo_windows_2.gif) [![WindowsMobileDeviceCenter](WindowsMobileDeviceCenter_thumb.png "WindowsMobileDeviceCenter")](https://kapie.com/content/binary/WindowsLiveWriter/Windows7andWindowsMobilePhone_11776/WindowsMobileDeviceCenter_2.png) After upgrading to [Windows 7](http://www.microsoft.com/windows/windows-7/default.aspx) on my laptop I found that I could no longer sync with my Sony Ericsson X1 (Windows Mobile 6.1) Phone. It didn’t even seem to be charging (over USB).

Looking in the system Device Manager I found a missing driver for the ‘Generic RNDIS’ device.

A bit of goggling uncovered that this was something required for syncing mobile devices. Although there are comments around that Windows Mobile devices are not supported on Windows 7 beta, and a number of people seem to be having the same problem, the good news is it does actually work.

I simply downloaded the [Windows Mobile Device Center 6.1 for Vista (from Microsoft)](http://www.microsoft.com/windowsmobile/en-us/help/synchronize/device-center.mspx), installed it and everything was rosy.

It installed the driver for the ‘Generic RNDIS’, I connected the phone, it was recognised a Microsoft USB Sync device was installed and it all started working as expected.

GEO 51.4043694884482:\-1.28756761550903