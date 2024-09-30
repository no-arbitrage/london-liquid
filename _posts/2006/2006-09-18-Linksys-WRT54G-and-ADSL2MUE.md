---
title: "Linksys WRT54G and ADSL2MUE"
date: "2006-09-18T12:37:38"
tags: [
  "technical"
]
---
The [Linksys WRT54G](http://www.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=US%2FLayout&cid=1149562300349&packedargs=site%3DUS&pagename=Linksys%2FCommon%2FVisitorWrapper) is a wireless 4 port router / firewall. It has 4 wired Ethernet ports, a Wireless Access Point and router/firewall functionality. It does NOT have a DSL modem built in (so it will NOT connect to your phone line to link it up to your BTBroadband, Talk Talk Broadband or any other Broadband account.  
It is a small box with a little computer inside running a customized version of Linux.

Until recently I was running this plugged into a DLink DSL-300T DSL Modem (this does the conversion from DSL to Ethernet and passes the data to/from the [WRT54G](http://www.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=US%2FLayout&cid=1149562300349&packedargs=site%3DUS&pagename=Linksys%2FCommon%2FVisitorWrapper) box). The DLink modem failed so, when replacing it I immediately looked to Linksys (as I have been very happy with the [WRT54G](http://www.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=US%2FLayout&cid=1149562300349&packedargs=site%3DUS&pagename=Linksys%2FCommon%2FVisitorWrapper)).  
I found they do a [ADSL2MUE](http://www-uk.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=UK%2FLayout&cid=1122994880477&packedargs=sku%3D1122994880477&pagename=Linksys%2FCommon%2FVisitorWrapper) box which is a pure DSL Modem and it stacks really well on top of the [WRT54G](http://www.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=US%2FLayout&cid=1149562300349&packedargs=site%3DUS&pagename=Linksys%2FCommon%2FVisitorWrapper), also it’s only about £22 so it was a done deal.

I’m fairly comfortable linking these together configuring it as necessary, so I was really surprised to see the amount of people having difficulty when I looking for reviews of the [ADSL2MUE](http://www-uk.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=UK%2FLayout&cid=1122994880477&packedargs=sku%3D1122994880477&pagename=Linksys%2FCommon%2FVisitorWrapper).

Most ‘getting you started’ guides seem to outline how to get the box working on it’s own but never ‘how it all works in a typical environment’. So here’s my comments on how you go about it in a generic manner (any router and DSL modem)…

**So, how do I configure them to work together.**

Both units come with a default IP address of 192.168.1.1, also, both units have a web interface to allow configuration. So… you do NOT want to plug them both into a switch/hub at once as you’ll get an IP address conflict.

So, take the **modem** and plug it into the switch/hub. Configure your PC to have a **static** IP address of 192.168.1.2 (Actually the router provides DHCP by default, so you could leave your PC configured to Dynamically Assigned, but just in case…)  
Open your web browser and enter the address 192.168.1.1 – enter your username and password (by default admin & admin) and your in.

Now change the network settings so that your modem has an IP address of 192.168.2.1  
When you commit the changes you will no longer be able to access the web interface – so change your PC to a static IP address of 192.168.2.2  
Configure your modem to connect to your ISP as usual. Also configure the modem to provide DHCP to the Ethernet clients  
Now we have the modem working we need to connect it up to the router and get that configured.

So, take the **router** and plug it into the switch/hub. Configure your PC to have a **static** IP address of 192.168.1.2 (Actually the router provides DHCP by default, so you could leave your PC configured to Dynamically Assigned, but just in case…)  
Open your web browser and enter the address 192.168.1.1 – enter your username and password (by default admin & admin) and your in.

Now configure the router to use Automatic IP Configuration on the WAN side. Configure your normal router settings and your all set.

**I’ve done that, but what is actually happening.**

OK, your PC  has been automatically assigned an IP address in the range 192.168.1.X When you try to make a Internet connection (Web, Instant Messenger, Online games etc) then it is trying to connect to an IP address (lets say 80.12.23.45) your PC cannot connect to that IP address (it can only connect to other machines in the 192.168.1.X range) so instead it sends the request to the default gateway (which is your router).  
Your router does all it’s filtering and firewall stuff and eventually (if allowed) the request goes to the WAN port of the router – this is connected to the modem and has been assigned an IP address automatically (lets say 192.168.2.2, because that’s the range it’s in) the router forwards it to the modem which passes it through to the DSL connection and on to your ISP etc etc until it gets to the right location.

**Points To Note**

The router and modem MUST be on separate subnets (i.e. 192.168.1.X and 192.168.2.X – it doesn’t have to be these address but they MUST be different – at least one of the first 3 sets of digit must differ)

This post outlines how to configure the modem in MODEM mode, not BRIDGED mode – that is a different discussion (email me directly)