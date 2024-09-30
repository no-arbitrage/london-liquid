---
title: "Updating a ADSL2MUE"
date: "2006-09-19T09:06:43"
tags: [
  "technical"
]
---
I recently purchased a [Linksys ADSL2MUE](http://www-uk.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=UK%2FLayout&cid=1122994880477&pagename=Linksys%2FCommon%2FVisitorWrapper), I was a bit dubious of it at first as it seemed to take forever to connect and the connection was very flaky.  
Also the configuration options were pretty poor in the 4.12UK version of software.

I then found that in the 4.12UK version of software it provides NAT for outgoing connections but drops ALL anonymous Internet requests – meaning I cannot initiate a connection for a remote machine to my home network – ***NOT GOOD*** as I like to have access to my files (FTP), my source code (Subversion) and my desktop (VNC).

After much googling and a very close call with parting with more cash for another DSL modem, I came across an Australian version of the firmware for this box that apparently enabled many more features.

Why not give it a go, I was planning on getting something else anyway, so if I ‘brick’ the device I’m still no worse off.

I found the firmware and a post about it [here](http://forum.emule-project.net/index.php?showtopic=93984&hl=adsl2mue.). The guy posting about has the files hosted on esnips, which requires a login etc, so to make it easier I’ve posted them [here](https://kapie.com/content/binary/adsl2mue.zip) also.

This new firmware is actually BETA (I think) and designed for the Australian market but hey – it works like a charm and gave me lots of new features ***AND A MUCH MORE STABLE CONNECTION***.

I now have it configured to pass all ports through seamlessly and all the firewall rules and port forwarding is handled by my Linksys [WRT54G](http://www.linksys.com/servlet/Satellite?c=L_Product_C2&childpagename=US%2FLayout&cid=1149562300349&packedargs=site%3DUS&pagename=Linksys%2FCommon%2FVisitorWrapper)G (running [DD-WRT](http://www.dd-wrt.com/dd-wrtv2/index.php) v23.SP2 firmware).  
So, full access to my home services, remote management capability and many more diagnostic facilities… ***Exactly what I need.***

 **Some Screenshots**

[![](CropperCapture1_thumb1.jpg)](https://kapie.com/content/binary/UpdatingaADSL2MUE_C094/CropperCapture13.jpg)

[![](CropperCapture3_thumb.jpg)](https://kapie.com/content/binary/UpdatingaADSL2MUE_C094/CropperCapture32.jpg)

[![](CropperCapture4_thumb.jpg)](https://kapie.com/content/binary/UpdatingaADSL2MUE_C094/CropperCapture42.jpg)