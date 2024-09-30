---
title: "Getting Started with a Pi Zero"
date: "2016-06-29T14:11:06"
tags: [
  "beginner"
]
image: assets/images/getting-started-with-a-pi-zero-IMG_20160629_130559_thumb.jpg
---
Managed to get hold of a Pi Zero ? Here’s how to get started with it…

The Pi Zero only has a single micro USB port for connecting peripherals. That can make it a little tricking when trying to get things set up. There’s a few bits that you will need to go along with your Pi Zero in order to set it up and get it going :

-   A way to power it (I use [this portable battery power pack](http://amzn.to/292CzmY), but you can just use a charger plug, or computer USB port).
-   A USB Wi-Fi dongle (I [use the official one](http://amzn.to/2948LnE), but you can use any supported one).
-   A micro SD card ([£3.50 gets you a 16GB one](http://amzn.to/293lIwX) these days, and for a couple of extra quid you can get 32 or 64 GB cards).
-   A [micro USB OTG adapter](http://amzn.to/292ut9W).
-   A [mini HDMI to HDMI cable](http://amzn.to/294LkOT).
-   A [wireless keyboard and mouse combo](http://amzn.to/29496GZ).

First up you’ll want to load the latest version of [Raspbian](https://www.raspbian.org/) on to your micro SD card (instructions here), then you’ll want to insert the micro SD card into your Pi Zero, making sure it’s pushed all the way in.

![IMG_20160629_132625](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132625_thumb.jpg)
![IMG_20160629_132649](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132649_thumb.jpg)

Next, connect up the micro USB OTG (On The Go) connector to the wireless keyboard/mouse dongle and insert that into the middle of the three connectors.

![IMG_20160629_132807](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132807_thumb.jpg)
![IMG_20160629_132820](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132820_thumb.jpg)
![IMG_20160629_132849](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132849_thumb.jpg)

Lastly, for this bit, connect up the mini HDMI cable to the monitor and then connect the micro USB power cable.

![IMG_20160629_132931](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132931_thumb.jpg)
![IMG_20160629_132954](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_132954_thumb.jpg)

The Pi Zero should now boot and you’ll see the text boot sequence log flashing by on the monitor. Depending on which version of [Raspbian](https://www.raspbian.org/) you are using you will either get a shell (text based console) or the screen will blank and then start up the desktop. If it boots to the desktop, then open xTerminal, if it boot to the shell then your good to go.

What we need to do now is to configure the Wi-Fi settings. To do this enter the following :

```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

This will open the existing file which will likely only have 2 lines in it. Underneath those lines you will add your Wi-Fi details. It looks like this:

```
network={  
    ssid=”MYSSID”  
    psk=”mypassword”  
    id_str=”Home”  
}
```

Replace **MYSSID** and **mypassword** with the actual values for your Wi-Fi network.

You can multiple of these ‘network’ sections if you have multiple Wi-Fi access points that your Pi Zero will access. Just make sure that each of them has a unique id\_str.  
When you have added your details hit CNTL+X, then Y and then ENTER to quit and save the file. Now you can shutdown the Pi Zero and we’ll do a little ‘plug and play’

When it has shutdown pull out the wireless keyboard/mouse dongle (it may leave the OTG adapter in there, no problem). Now plug in the USB Wi-Fi adapter to the OTG and make sure it’s plugged into the Pi Zero. Reapply power (unplug and plug in again) and it should boot up as usual, but will now automatically connect to Wi-Fi.

![IMG_20160629_133031](/assets/images/getting-started-with-a-pi-zero-IMG_20160629_133031_thumb.jpg)

You can now access it from a remote machine via SSH. Open your SSH client and connect to raspberrypi.local (note: you need to have iTunes or other Apple software installed on the remote machine for it find the Pi using .local) Assuming all that has worked you’ll have a SSH connection to your Pi Zero and can tinker as needed. You can also, at this stage, remove the mini HDMI and run ‘headless’.

Great, we’re done, one Pi Zero set up with Wi-Fi, running headless, ready for some project.