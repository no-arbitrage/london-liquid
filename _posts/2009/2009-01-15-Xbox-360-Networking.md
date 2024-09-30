---
title: "Xbox 360 Networking"
date: "2009-01-15T02:34:54"
tags: [
  "hardware",
  "technical"
]
---
[![xbox360](xbox360_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/Xbox360Networking_12322/xbox360_2.jpg) So before Christmas I bought an Xbox 360 (60GB HDD version). Reasoning was “it will be good for the kids hand eye coordination” which in reality was a thinly veiled “I want one and the kids might like it too” (as it turns out they don’t)…

Anyway, one of the first tasks was networking it -linking it to the home PC and DSL router. I read a bunch of stuff online about the Xbox wireless adapter being over the top and the same could be done with any old wireless router that you had lying around around – so, having an old WRT54G lying around that was my first step.[![wrt54g](wrt54g_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/Xbox360Networking_12322/wrt54g_2.jpg)

It was pretty successful in that I already had reflashed it with DD-WRT firmware and it was pretty simple getting the wireless side acting as a client to my Linksys WAG160N and the unit bridging connections from the LAN side. Now this configuration requires that you set the LAN side side up on a different subnet to the existing (wireless) network – otherwise the routing gets screwed up. i.e. The LAN side might be on 192.168.2.X while the existing router is on 192.168.1.x (this is what routing is all about – Xbox gets an address of 192.168.2.x and a default GW of 192.168.2.1, the router bridges any data sent to 192.168.2.1 over to the wireless side which has an address of 192.168.1.xxx and a default GW of of 192.168.1.1 that then bridges it over to the WAN side of the DSL connection that then routes it to the Internet…

This worked fine initially, I could connect to Xbox Live and the like, but where it fell down was on the connection to the Media Centre PC – for some reason any Media Centre PC must be on the same segment as the Xbox (which is not the case here as Xbox = 192,168.2.x and MCPC = 192.168.1.x). It does this to make ‘discovery’ of the MCPC easy fro an Xbox, but I am surprised (read disappointed) that there is not an advanced / manual setting that allows the user to specify the IP address. I reckon this is around needing a good quality network connection between the devices (which is generally the case if they are on the same subnet, but not always when on different subnet’s), but a simple disclaimer could have sufficed…. It forces users into either running an Ethernet cable, buying a wireless adapter or living without media centre capability.

[![xbox360wireless](xbox360wireless_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/Xbox360Networking_12322/xbox360wireless_2.jpg) Anyway I caved and bought an Xbox wireless adapter, but the speeds are still not up to streaming HD TV, so now I’m considering powerline adapters (Homeplug) – be glad to hear of any experiences you have with this…

GEO 51.4043197631836:\-1.28760504722595