---
title: "WindowsLive Writer Plugin"
date: "2006-09-21T11:51:15"
tags: [
  "technical"
]
---
Having raved about WindowsLive Writer recently,  I’ve been using it extensively over the past few days.

I’m using Dasblog 1.8 for this site and apparently it doesn’t support the API required to automatically upload images etc (Writer recognizes this and offers me FTP instead – which works well). That’s all fine and good for images, but sometimes I want to give links to files…

Writer provides a pretty easy API and plug in architecture so I thought I’d write my own Plugin which allows the user to browse for a file, upload it and insert a link to the file into the post content.

This was a really exercise, taking only about 2 hours total for coding and testing. You can get the binary files [here](https://kapie.com/content/binary/InsertFileViaFtp.zip) and the source code [here](https://kapie.com/content/binary/InsertFileViaFtp_Source.zip). This was really for my own use (and doesn’t do any error handling), but feel free to use it…

Basically, copy the binary files to the Plugins sub folder of your Writer folder and restart Writer, it should be automatically recognized and you’ll get a new item in the ‘Insert’ tab of the sidebar

[![](CropperCapture1.jpg)](https://kapie.com/content/binary/WindowsLiveWriterPlugin_AA20/CropperCapture11.jpg)

Click on the item and you get a dialog allowing you to specify the file to upload, which site to upload it to and the text to display as the link in the post.

[![](CropperCapture2_thumb1.jpg)](https://kapie.com/content/binary/WindowsLiveWriterPlugin_AA20/CropperCapture23.jpg)  

The XML file allows you to specify what sites you want to upload to. Specify the correct settings for your FTP server / path.  
Only one item should have ‘default’ set to true (this makes it the default selected item in the ComboBox) and if you have ‘debug’ set to true then you will get a messagebox indicating the Uri it is trying to send the file to.