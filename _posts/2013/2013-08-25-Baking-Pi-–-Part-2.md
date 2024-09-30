---
title: "Baking Pi – Part 2"
date: "2013-08-25T23:26:39"
tags: [
  "hardware",
  "raspberry-pi",
  "technical"
]
---
![](https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcRNSrGI_pllKIBb5ulBIBXflWGj9OLV9-4OnnDzUa_9pfgyrJ-t)

The first part of this ‘getting things up and running’ series [can be found here](https://kapie.com/2013/baking-pi-part-1).

In this post I wanted to outline what was required to set up Wi-Fi and to get a Microsoft LifeCam 6000 working, providing a web page with the camera image streaming.

So, Wi-Fi… I bought a £6.99 USB Wi-Fi dongle from eBay. After plugging it in and rebooting the Pi I opened a SSH session to it and typed ‘***lsusb***’ This lists all the usb devices, and I could see the Wi-Fi adapter in the list as a Ralink RT5370.

First thing was to get the drivers – doing an ‘***apt-cache search ralink***’ found me the correct package (firmware-ralink). On issuing ‘***apt-get install firmware-ralink’*** it told me that the version I had already up to date – great it seems the Raspbian ‘Wheezy’ image comes with it installed.

So, it was just a case of setting the Wi-Fi options and giving it an IP address (static). I do this directly in the interfaces file. So…

-   From the command line run ***sudo nano /etc/network/interfaces***
-   Change "***iface wlan0 inet dhcp***" to "***iface wlan0 inet static***"
-   below this add…
-   ***address 192.168.97.15***
-   ***netmask 255.255.255.0***
-   ***gateway 192.168.97.1***
-   also add the following Wi-Fi options
-   ***wpa-ssid YOUR\_SSID***
-   ***wpa-psk YOU\_KEY***
-   ***wireless-power off***
-   Now reboot (**sudo reboot**)

You should now have a Wi-Fi enabled Pi.

[![lifecam6000](lifecam6000_thumb.jpg "lifecam6000")](https://kapie.com/wp-content/uploads/2013/08/lifecam6000.jpg)For the webcam, things were a little trickier… A lot of people are using ‘**motion**’ for setting up security cameras, as it does motion detection and can spit out images or movies when some motion is detected as well as do time-lapse and provide a web based video stream for viewing in a browser. However, this was overkill for what I had in mind (just simple streaming of the video), and it also uses a lot of horsepower.  
So instead I selected [mjpg-streamer](http://sourceforge.net/projects/mjpg-streamer/), an open source project hosted on SourceForge.

There are no prebuilt binaries for the Pi, so it’s a case of building it yourself – not too difficult…

First get all the dependencies…

-   ***sudo apt-get install libv4l-dev***
-   ***sudo apt-get install libjpeg8-dev***
-   ***sudo apt-get install imagemagick***

I tried installing subversion to check out the code, but the svn urls have moved around and it wouldn’t let me do a checkout as it couldn’t find the correct url, so instead I just downloaded the zipped tarball and extracted it…

-   ***wget*** [***http://sourceforge.net/projects/mjpg-streamer/files/latest/download***](http://sourceforge.net/projects/mjpg-streamer/files/latest/download)
-   ***mv download mjpg-streamer-r63.tar.gz***
-   ***tar –vzxf mjpg-streamer-r63.tar.gz***
-   ***cd mjpg-streamer-r63***

At this point I tried to do a ‘make’ but it failed stating it could not find linux/videodev.h. A bit of noodling around found that I had a videodev2.h file, so all that was needed was a symbollic link.

-   ***ln –s /usr/include/linux/videodev2.h /usr/include/linux/videodev.h***

Now to build it…

-   make clean all

I did get a few error towards the end, but the key elements built correctly (I think it was just a plugin or two that failed to build, so I simple glossed over that).

Now you can start the application manually with the following command line:

-   ***./mjpg\_streamer -i "./input\_uvc.so" -o "./output\_http.so -w ./www"***

.. but what we really want is to start it automatically when the Pi boots so…

-   ***sudo /etc/init.d/webcam***

… and add the following text to it…

```

### BEGIN INIT INFO
# Provides: mjpg-streamer
# Required-Start: networking
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Starts mjpg-streamer
# Description:
### END INIT INFO

#! /bin/sh
# /etc/init.d/webcam

# Start / Stop the webcam streamer
case "$1" in
  start)
    echo "Starting webcam streaming"
    /home/pi/mjpg-streamer-r63/mjpg_streamer -o "/home/pi/mjpg-streamer-r63/output_http.so -w /home/pi/webcam/mjpg-streamer-r63/www" &
    ;;
  stop)
    echo "Stopping webcam streaming"
    killall mjpg_streamer
    ;;
  *)
    echo "Usage: /etc/init.d/webcam {start|stop}"
    exit 1
    ;;
esac

exit 0
```

Now the final touches of making it executable and making sure it get started…

-   ***sudo chmod 755 /etc/init.d/webcam***
-   ***update-rc.d webcam defaults***

.. and the final part is of course viewing your handiwork – so open a browser and type [:8080">http://<your\_pi\_ipaddress>:8080](http://<your_pi_ipaddress>:8080) and you should be able to see the webcam image.