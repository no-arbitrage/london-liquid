---
title: "SMS Gateway follow up"
date: "2009-01-09T22:29:30"
tags: [
  "development"
]
---
I’ve had a lot of interest in my SMS Gateway app since [this post](https://kapie.com/2008/10/02/SMSGatewayUsingWM6AndActiveSync.aspx). SMS gateway consist of two components :

**SMS Gateway Addin[![image](image_thumb_1.png)](https://kapie.com/content/binary/WindowsLiveWriter/SMSGatewayusingWM6andActiveSync_13EA5/image_4.png)  
**This is an Outlook addin that adds a new toolbar to your Outlook instance. The toolbar allow the user to choose a mobile / cellphone number (from their contacts) and enter a message. When they hit the enter key after the message (or click ‘Send’) a new Task is created when has a subject of ‘SECRETCODE’ and the mobile / cellphone number and the details being the text of the message.

At some stage this Outlook Task is synchronized (over ActiveSync / Direct Push) to a Windows mobile device…

**SMSGateway  
**This is a small app running on a Windows Mobile device that every 15 seconds checks through the tasks. If it finds any tasks that have a subject beginning with SECRETCODE then it parses out the mobile / cellphone number and sends the message text (from the Task details) to that mobile / cellphone number via SMS. ***Note:** the SECRETCODE word is configurable.*

**[![image](image_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/SMSGatewayusingWM6andActiveSync_13EA5/image_2.png)Why develop this ?  
**The purpose of this app was really to allow me to send SMS messages easily from Outlook without having to sign up for (and pay for) a web to SMS service (I already get 100’s of free SMS messages with my mobile / cellphone package).

The application is free for anyone to use (drop me a line – in the comments – if you do use it…)

Windows Mobile CAB file (copy the file to your Windows Mobile device and click on it) : [SMSGatewayMobile.CAB](https://kapie.com/projects/smsgateway/smsgatewaymobile.cab)  
Setup file for the Outlook 2003 Add-in (Outlook 2007 coming soon) : [SMSGatewayAddin2003.msi](https://kapie.com/projects/smsgateway/smsgatewayaddin2003.msi)  
Source for both the applications : [SMSGateway.zip](https://kapie.com/projects/smsgateway/smsgateway.zip) (includes test Outlook 2007 addin code)

I’d be really interested to hear from anyone using this…. post in the comments…

GEO 51.4043197631836:\-1.28760504722595