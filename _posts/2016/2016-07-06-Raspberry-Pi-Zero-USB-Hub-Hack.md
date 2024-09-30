---
title: "Raspberry Pi Zero USB Hub Hack"
date: "2016-07-06T20:19:00"
tags: [
  "featured"
]
image: assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160706_201802_thumb.jpg
---
As much as we all love the Pi Zero, the connectivity options are **very** limited. With only one (micro) USB port (for peripherals) it means you need a [micro <=> ‘normal’ USB adapter](http://amzn.to/29kp6Y7) to get any of the usual accessories (keyboard, Wi-Fi dongle etc.) plugged in, and when you have that’s it – nothing else can be plugged in. Also, those [little USB adapters](http://amzn.to/29kp6Y7) just look a little bit to precarious for my liking. So, what I needed was a ‘Raspberry Pi Zero USB Hub Hack’.

I’d seen that the USB power and data lines are presented as pads on the board itself. I wasn’t alone in noticing this, and seemingly a bunch of people had success soldering on USB hubs directly. So I though I’d give it a go. I bought myself this [little ‘no name’ 4 port USB2 hub from eBay](http://www.ebay.co.uk/itm/390567004512) for the princely sum of £2.09 (including delivery) – how they make *any* profit on these is beyond me.

![IMG_20160705_201003](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160705_201003_thumb.jpg)
![IMG_20160705_201035](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160705_201035_thumb.jpg)

As soon as it arrived, I pulled the case apart exposing the board itself and cut the USB cable off. (NOTE: I probably should have tested it on the laptop, or Pi first, but I was a bit too eager).

![IMG_20160705_201158](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160705_201158_thumb.jpg)
![IMG_20160705_201530](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160705_201530_thumb.jpg)

A simple soldering job had the 4 individual cables soldered to their respective pads on the Pi Zero. It wasn’t a great success to start with, as it was ‘hit and miss’ whether it showed up on the Pi after booting.

![IMG_20160706_201802](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160706_201802_thumb.jpg)
![IMG_20160706_201856](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160706_201856_thumb.jpg)

I guessed that it was likely that not enough power was being delivered, to the hub, by the super thin cables. I decided to remove those completely and replace themwith some thicker cable to make sure there was enough power to the hub. I also used some thicker signal wire for the Data + and –.

![IMG_20160707_134138](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134138_thumb.jpg)
![IMG_20160707_134146](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134146_thumb.jpg)

However, now it never showed up, not even for a short time.

`lsusb`

![lsusb_no_hub](/assets/images/raspberry-pi-zero-usb-hub-hack-lsusb_no_hub_thumb.png)

It looked like it was recognising that a hub was plugged in, but it saw it as a ‘new low-speed USB device’. The seller on eBay had advertised this as a high-speed hub, so the ‘low-speed’ was odd. I would also get a lot of `device descriptor read/64, error -71` errors in dmesg when the hub did not show up.

`dmesg | grep usb`

![dmesg_usb](/assets/images/raspberry-pi-zero-usb-hub-hack-dmesg_usb_thumb.png)

Googling around suggested trying out a bunch of dwc_otg settings in cmdline.txt, but none of those seemed to work. I also switched it to low speed (`dwc_otg.speed=1`) and that didn’t help. I knew all the connects were good and there were not shorts, so I began to think I had killed the IC on the hub somehow.  
A last ditch attempt was to swap around the Data + – wires. I didn’t expect this to work as it was being ‘seen’ by the Pi, just not working. I figured, how could it ‘see’ the hub without being able to read ‘something’ from it. WRONG….

Swapping round the Data + – wires brought it to life, every time I powered on the Pi. Excellent. So now a little tidying things up, getting some anti-static foam between the boards and squishing them together with and elastic band (temporary) got me to the finish line.

`lsusb`

![lsusb_with_hub](/assets/images/raspberry-pi-zero-usb-hub-hack-lsusb_with_hub_thumb.png)

`dmesg | grep usb`

![dmesg_with_hub](/assets/images/raspberry-pi-zero-usb-hub-hack-dmesg_with_hub_thumb.png)

Here are some pictures of the finished Raspberry Pi Zero USB Hack project (although it still needs tidying and a case etc.)…

![IMG_20160707_134618](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134618_thumb.jpg)
![IMG_20160707_134629](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134629_thumb.jpg)
![IMG_20160707_134645](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134645_thumb.jpg)
![IMG_20160707_134715](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134715_thumb.jpg)
![IMG_20160707_134725](/assets/images/raspberry-pi-zero-usb-hub-hack-IMG_20160707_134725_thumb.jpg)

Next up is to replace one of the hub ports with a directly soldered Wi-Fi dongle. Watch this space…