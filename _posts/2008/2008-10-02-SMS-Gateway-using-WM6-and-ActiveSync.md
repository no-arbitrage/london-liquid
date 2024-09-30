---
title: "SMS Gateway using WM6 and ActiveSync"
date: "2008-10-02T21:40:26"
tags: [
  "development"
]
---
[![image](image_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/SMSGatewayusingWM6andActiveSync_13EA5/image_2.png)

Recently I have wanted to be able to send SMS messages to my contacts without having to pick up my phone and mess around with the mini keyboard (I wanted to do it directly from PC, but have the message still send by the phone…

There are solutions around that allow you to send a SMS from a form on a web page, but these are generally paid for services or limited to XX messages per day – I have a mobile contract with unlimited texting, so why would I want to pay for another service, and I didn’t want to be limited in the volume of texts I can send per day. there are also solutions around that require the device to be docked / plugged into the PC, I didn’t like this either as it’s just too much hassle (and therefore I never do it)…

So I came up with a simple but effective solution that makes use of ActiveSync / push email technology that is built into Windows Mobile devices.

Basically I wrote an Outlook Addin that allows me to choose a contact from a drop down list, type in the message and “send” it – when I say “send” it, what I mean is the request is transferred to the WM6 device which then sends the actual SMS message.

[![image](image_thumb_1.png)](https://kapie.com/content/binary/WindowsLiveWriter/SMSGatewayusingWM6andActiveSync_13EA5/image_4.png)

So it goes like this :-[![image](image_thumb_4.png)](https://kapie.com/content/binary/WindowsLiveWriter/SMSGatewayusingWM6andActiveSync_13EA5/image_10.png)

-   User chooses the contact from a  drop down list of all contacts with mobile numbers.
-   User enters the text of the message
-   User clicks send
-   Outlook Addin creates a new task with a secret keyword followed by the mobile number as the subject and the message in the body of the task.
-   (after a few seconds) the new task is synchronized to the WM6 device via push email / ActiveSync
-   WM6 device regularly checks the tasks list for a task with a subject that starts with the secret keyword
-   The subject line is parsed to get the mobile number the text is to be sent to
-   The body is parsed to get the text of the message
-   A SMS message is send from the WM6 device.
-   The new task is deleted (or marked as complete)
-   The change to the task (delete or marked as complete) is synchronized back to the PC via ActiveSync / push email.

I’ll have more details, the installers and all the source code available next week…

***UPDATE: See the follow up to this article*** [***here***](https://kapie.com/2009/01/09/SMSGatewayFollowUp.aspx) ***which includes binary files and full source code.***

GEO 51.4043197631836:\-1.28760504722595