---
title: "Raspberry Pi P6 Reset Hack 2"
date: "2016-06-28T23:00:39"
tags: [
  "advanced",
  "snippets"
]
image: assets/images/raspberry-pi-p6-reset-hack-PiP6WithHeaderPins_thumb.png
---
I’ve been trying to get some code running on my Pi 2. The code has been ‘problematic’ to get running, and I’m running into frequent hangs on the Pi where I cannot initiate a new SSH or console session. It’s also complicated because the Pi is outside in the garage (I have a GPS hooked up so it needs some visibility of sky). That means that every time it hangs I have to head out to the garage, pull the micro USB cable, reinsert it and then start the whole ‘Edit, Compile, Run, Head out to the garage to Reset’ cycle again.

Anyway, in an effort to simplify things I was looking around for a way of remotely resetting the Pi without having a connection to it. I found a ton of sites talking about the P6/RUN header on the newer boards, but this was mostly just about adding pins to short out with a jumper or adding a momentary switch to do the same. What I wanted was something more like connecting an Arduino pin, or a pin from another Pi to the P6/RUN on the target Pi and forcing a reset that way. What I wanted was a “Raspberry Pi P6 Reset Hack” – unfortunately AdaFruit don’t sell a kit for that.

![PiP6PullUpResistor_thumb](/assets/images/raspberry-pi-p6-reset-hack-2-PiP6PullUpResistor_thumb_thumb.png)

I was initially concerned about the current that the watchdog Pi would have to sink on the GPIO0 pin, but looking at schematics it seems the RUN pin is pulled up to 3.3V by a 10K resistor – a little bit of Ohms law says this will mean 3.3v/10K = 0.33mA will flow, well within the recommended limits of the GPIO pins 2mA to 16mA

I have an ESP8266-12 laying around just waiting for this kind of thing, but for the sake of speed I opted for another Pi I had (already hooked up, working and configured). So this essentially came down to :

-   Solder header pins on the target Pi (the one to be reset)
-   Connect the grounds of the target and ‘watchdog’ Pis together
-   Connect a GPIO pin from the ‘watchdog’ Pi (GPIO0) to the RUN pin on the target Pi
-   Putting together a small app to force the GPIO0 pin low for a few milliseconds and then high again.

![PiP6WithHeaderPins_thumb](/assets/images/raspberry-pi-p6-reset-hack-PiP6WithHeaderPins_thumb.png)
![Pi2P6WithGndAndGpioConnected_thumb](/assets/images/raspberry-pi-p6-reset-hack-2-Pi2P6WithGndAndGpioConnected_thumb_thumb.png)

For the app to be run on the ‘watchdog’ Pi, I’m making use of the excellent wiringPi library. Instructions for downloading and building it can be found at [http://wiringpi.com/](http://wiringpi.com/ "http://wiringpi.com/")  
The code for ‘reset-app’ is simply (nano reset-app.c):

```c
#define <wiringPi.h> 
int main (void) 
{
      wiringPiSetup();
      pinMode(0, OUTPUT);
      digitalWrite(0, HIGH);
      delay(500);
      digitalWrite(0, LOW);
      delay(500);
      digitalWrite(0, HIGH);
      return 0; 
}
```

… then to build it just execute :

```bash
sudo gcc –Wall –o reset-app reset-app.c –lwiringPi
```

Now when my target Pi hangs I can just ssh into the watchdog Pi, and run `sudo  ./reset-app`  and the target Pi reboots.  
Also, when I’m done with this testing/hanging stuff I may simply connect a switch to the header for ‘’future ‘just-in-case’ things…

**DISCLAIMERS**

This is a QuickAndNasty™ solution. I am not responsible for any damage to your Pi, your electrics, your health or anything else – anything you do as a result of reading this is at your own risk.  
Using this to reset you Pi can result in SDcard corruption !!   All calculations are off the top of my head, and could be wrong.

There are a ton of things to improve it also:

-   It relies on the GPIO pin on the watchdog ‘floating high’ while it is an input – it really should be set as an output (HIGH) at start-up.
-   I’ve not tested what happens when the watchdog Pi reboots – there’s a chance (likelihood?) that it will reboot the target Pi.
-   The ‘reset-app’ can be improved (drastically!!)

*EDIT: I have tested what happens when the watchdog Pi reboots and, for me, it does not reboot the target Pi – however there is a chance (likelihood ?) that it will reboot the target Pi.*  

Anyway – enjoy, and let me know if you found it useful….