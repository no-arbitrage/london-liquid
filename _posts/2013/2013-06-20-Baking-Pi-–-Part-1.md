---
title: "Baking Pi – Part 1"
date: "2013-06-20T22:22:06"
tags: [
  "hardware",
  "raspberry-pi",
  "technical"
]
---
![](https://encrypted-tbn1.gstatic.com/images?q=tbn:ANd9GcRNSrGI_pllKIBb5ulBIBXflWGj9OLV9-4OnnDzUa_9pfgyrJ-t)

After [starting with a Raspberry Pi](https://kapie.com/2013/starting-with-a-raspberry-pi) that was just too simple to set up as XMBC media centre for daughter #1 bedroom, it soon became a permanent feature there – meaning, of course, that I needed another…

I now have my second helping of Pi – again I got a [Raspberry Pi Model B (512MB RAM)](http://www.amazon.co.uk/gp/product/B009SMWSQA/ref=as_li_ss_tl?ie=UTF8&camp=1634&creative=19450&creativeASIN=B009SMWSQA&linkCode=as2&tag=configexchan2-21)![](http://www.assoc-amazon.co.uk/e/ir?t=configexchan2-21&l=as2&o=2&a=B009SMWSQA).

I’m running this mostly headless and wanted to post a few pointers on my setup (so I can recall it when I trash the Raspbian OS and have to restart from scratch.

After a standard Raspbian install I am doing the following actions / configurations :

1.  Basic configuration via **raspi-config**
2.  Setting a static IP address
3.  Updating all packages
4.  Adding a custom port to listen for SSH on (for remote access through home router)
5.  Setting up **vsftpd**

Here is the step by step guide:

Basic configuration via raspi-config

-   Make an SSH connection to the device and login (pi / raspberry)
-   From the command line run ***sudo raspi-config***
-   Upgrade raspi-config
-   Configure as required.

Setting up a static IP address

-   From the command line run ***sudo nano /etc/network/interfaces***
-   Change “***iface eth0 inet dhcp***” to “***iface eth0 inet static***“
-   [![image](image_thumb.png "image")](https://kapie.com/wp-content/uploads/2013/06/image.png)below this add…
-   ***address 192.168.97.12***
-   ***netmask 255.255.255.0***
-   ***gateway 192.168.97.1***
-   Now reboot (**sudo reboot**)

Updating all packages

-   From the command line run ***sudo apt-get update***
-   From the command line run ***sudo apt-get upgrade***
-   Now reboot (**sudo reboot**)

Adding a customer port to listen for SSH on

-   From the command line run ***sudo nano /etc/ssh/sshd\_config***
-   Add a line under where it says ***Port 22***
-   Type ***Port xxxx*** on the new line (where xxxx is your desired additional port number)

Setting up vsftpd (FTP Server)

-   From the command line run ***sudo apt-get install vsftpd***
-   Now edit the config file to change the port is listens on
-   From the command line run ***sudo nano /etc/vsftpd.conf***
-   Under the line that reads ***listen=YES*** add the following lines
-   ***listen\_port=xxxx*** (where xxxx is your desired port)
-   ***pasv\_enable=YES***
-   ***pasv\_min\_port=yyyyy*** (where yyyyy is the lower range of ports you want it to use)
-   ***pasv\_max\_port=zzzzz*** (where zzzzz is the upper range of ports you want it to use)
-   Now restart the vsftpd service with ***sudo /etc/init.d/vsftpd restart***

All done. The Pi is now configured to allow SSH and FTP access on custom ports (with corresponding holes through the firewall to allow external access). Enjoy…