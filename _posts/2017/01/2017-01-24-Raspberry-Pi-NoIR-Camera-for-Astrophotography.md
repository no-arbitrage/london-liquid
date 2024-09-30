---
title: "Raspberry Pi NoIR Camera for Astrophotography"
date: "2017-01-24T17:25:51"
tags: [
  "blog"
]
---
I’ve been looking at using my Raspberry Pi NoIR Camera for Astrophotography recently…

A decent shot of the Milky Way is the objective. A decent time-lapse video of a clear sky through the night is a secondary goal. The Pi NoIR camera may be a bit limited as it only has up to around 6 seconds of exposure. This limitation can be countered with ‘stacking’ (if I understand it correctly).  
There is a great resource [here](http://www.lonelyspeck.com/astrophotography-101/) that dives into the detail of how best to shoot astrophotography. It covers some great guides as to what equipment, what settings etc.  
There are some interesting points around settings used for the Pi Camera with raspistill to get ‘star trails’ in [this project](http://www.mccarroll.net/blog/startrails/index.html). Those settings are a good starting point.

There is a lot of trial and error involved in shooting this kind of stuff. Capturing a bunch of frames, stacking and seeing how they look, rinse, repeat.  
To hopefully remove a lot of that trial and error, I’ve used this [github project](https://github.com/silvanmelchior/RPi_Cam_Web_Interface). It has some really useful features:

-   A web view of the image, and
-   Allows control of all camera aspects, and
-   Handles time-lapse, and
-   Provides downloads of the acquired images, video or sequences.

So, tonight’s plan is to get things set up outside, find the optimal settings, then kick off a time-lapse run.

If I get a chance I’ll also look out my little [IR LED module](/IR-LEDs-for-a-Pi-NoIR-Camera) and see how things look with that.