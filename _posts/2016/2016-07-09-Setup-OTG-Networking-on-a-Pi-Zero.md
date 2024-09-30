---
title: "Setup OTG Networking on a Pi Zero"
date: "2016-07-09T20:57:25"
tags: [
  "information"
]
---
It is possible to get the micro USB ‘OTG’ port on the Pi Zero configured for networking. After a couple of bits of configuration you can simply plug a cable between your PC / Laptop and the Pi Zero USB data port, it automatically creates a network between them and you can then SSH to your Pi Zero.

To enable this you’ll need the following line added to your `/boot/config.txt` file:

```
dtoverlay=dwc2
```

… and the following text added to your `/boot/cmdline.txt` (be sure to keep only space between each param and no newlines) after the  `rootwait` parameter:

```
modules-load=dwc2,g_ether
```

That should be it complete. Reboot the Pi Zero and check it out.  
*NOTE: This will only work on a Pi Zero, none of the other versions support it.*

Thanks to [Andrew Mulholland for the pointers](http://blog.gbaman.info/?p=791).