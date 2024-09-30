---
title: "Raspberry Pi with a RDA5708 FM Radio Module"
date: "2016-10-08T12:57:51"
tags: [
  "blog"
]
image: assets/images/raspberry-pi-rda5708-fm-radio-module-IMG_20160928_195116-223x300.jpg
---
I’ve been playing around with getting a Raspberry Pi with a RDA5708 FM Radio module working.

![Raspberry Pi with a RDA5708 FM Radio](/assets/images/raspberry-pi-rda5708-fm-radio-module-IMG_20160928_195116-223x300.jpg "Pi RDA5807 FM Radio")

I bought 2 of them from ebay for around £7. They took around 2 weeks to arrive (from China).  
Wiring them up is pretty simple. They are controlled via I2C, so only need SDA, SCL and power (3v3 and Gnd). Audio out is via a standard 3.5mm headphone socket and it has pin holes for a 3 pin header providing Left, Right and Ground.

It took me a little while to get things going and working out the correct command sequences to send it. When that was cracked it was pretty much plain sailing. The resulting audio is fine (for a small radio). The headphones also act as a decent antenna for radio reception, so no need for connecting an antenna for testing.

Raspberry Pi with a RDA5708 FM Radio : Getting it working
---------------------------------------------------------

I will post a fuller tutorial later but getting it working boiled down to 3 steps:

1.  Send the correct initialisation commands (***0x00, 0xC0, 0x02, 0x00, 0x00, 0x04, 0x00, 0xC3, 0xA7, 0x60, 0x00***, wait 500mS then send ***0x00, 0xC0, 0x01***)
2.  Set the desired frequency and set the tune bit (there is a specific algorithm for it, but it is pretty simple to implement).
3.  Set the desired volume (least significant nibble of one of the registers)

There is a bunch more to do in terms of measuring signal strength, scanning frequencies etc. That said, for quickness, the 3 steps above will get you listening to your favourite station. Some of these modules also have RDS capabilitiy but I do not think the particular version I got has this. It may have and I just don’t yet know how to turn it on – more experimenting required.

Sample code will be posted to GitHub in the next few days [is on GitHub](https://github.com/kjhughes097/pi-rda5708)

I have a project in progress where I’m [replacing the ‘guts’ of a vintage Roberts Rambler II Radio](/Vintage-Pi-Radio-Project/) (from the 70’s) with this module.