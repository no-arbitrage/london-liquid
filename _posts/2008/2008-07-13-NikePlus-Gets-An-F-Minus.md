---
title: "NikePlus Gets An F Minus"
date: "2008-07-13T21:57:15"
tags: [
  "hardware",
  "technical",
  "web"
]
---
One of the things I find really helpful in terms of motivation (for running) is having easy visibility of weekly, monthly and total mileages, times, paces etc.

[![nikeplus_sportband](nikeplus_sportband_thumb.jpg)](https://kapie.com/content/binary/WindowsLiveWriter/NikePlusGetsAnFMinus_142A5/nikeplus_sportband_2.jpg) So, a couple of weeks ago I bought myself a [Nike+ Sportband](http://www.sweatshop.co.uk/Details.cfm?ProdID=3195&category=5). This promised it all – a senor that fits in your shoe and automatically records your distance, time and speed and wirelessly transmits it to a wristband. The wristband also has a detachable USB connector / screen that you simply plug into your PC and the data is auto uploaded to the [Nike+ web site](http://www.nikeplus.com/).

For too long I have been using an Excel spreadsheet and manually copying it between PCs, in the past couple of months I had started getting to grips with WCF by putting together a running log application that I had planned to host on my web site (just for me) – no need for that any longer, Nike+ was going to solve all my problems and more…

**How wrong I was**. I cannot tell you (although that is exactly what this post is trying to do) how technically inept this product is, not just the wristband but also the web site, the whole experience in fact.

[![coding-horror-official-logo-small](coding-horror-official-logo-small_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/NikePlusGetsAnFMinus_142A5/coding-horror-official-logo-small_2.png) If you are familiar with the class book [‘Code Complete’](http://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670/ref=pd_bbs_sr_1?ie=UTF8&s=books&qid=1215981578&sr=8-1) you will know about a ‘Coding Horror’ (things you really should not do when writing software), [Jeff Atwood](http://www.codinghorror.com/) even used the term for the name of his great blog – well this isn’t *all* software, so lets call them ‘Design Horrors’.

The first ‘Design Horror’ comes from trying to invent a new and snazzy way to secure a piece of technology to your wrist. Watch straps have been around for hundreds of years, everyone knows how to use it right ?, the only advancement in wrist fastening / securing technology ***ever*** was to use a Velcro patch to secure the two separate lengths of strap together (typically in sports pieces, for added speed). Did this stop the Nike engineers, no way – they came up with a new paradigm in wrist strap technology. One length of strap has 10 small holes, spaced by about 2 or 3 mm, the other length has two stud like protrusions that (using one hand) you must line up with the required two holes and push into place with a force just less than that required to push your thumb through the flesh of your wrist.

‘Design Horror’ 2 (DH2) is similar to that of DH1, remember the detachable USB connector / screen thing I mentioned, well to secure it the USB connector pushing into a slot in the wristband and then it is secured by one of these studs pressed into a hole, however the button on the face of the screen is exactly above that stud and pushing the USB thingy down to make sure it is secured typically results in the button being forcibly pressed for a few seconds (resulting in the device trying to locate the shoe sensor), also I’m not comfortable with the amount of pressure placed upon the button this regularly (thumb through the wrist pressure)….

[![nikeplus_logo](nikeplus_logo_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/NikePlusGetsAnFMinus_142A5/nikeplus_logo.png) ‘Design Horror’ 3 (DH3) is the ‘clock’ facility of the wristband. I may be being unfair here, this could be a ‘by design’ issue that was never part of the requirements (which would make it a Product Requirements Horror instead). The wristband has the ability to display the current time, as well as the mileages, pace etc. Naturally you would think that Nike position it as a watch replacement for runners, however I cannot believe that is the case. how could they imagine it would replace my watch which not only has alarms, date functions, countdown timers etc with something that only shows the time, nothing else, and does so without any form of backlighting, so that trying to read it outside the core hours of 10am to 4pm result in a painful headache and a trip to the optician for thick lenses. Obviously the decision to save what couldn’t be more than 10p, for a tiny surface mount device giving date/time features, in the cost of materials seemed important to them.

‘Design Horror’ 4 (DH4) is the orientation of the display. This is in the most difficult to read position available (regardless of how the thing is worn). The display does not read along the length/drop of your arm like most watches – no, it reads perpendicular to the length/drop of your arm. So, now, instead of just squinting at the display with no backlighting you are also skewing your arm / wrist into some crazy angle like a contortionist.

[![nikeplus_utility](nikeplus_utility_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/NikePlusGetsAnFMinus_142A5/nikeplus_utility.png) ‘Design Horror’ 5 (DH5) brings us to the client side software that you need on your PC to interface with the USB thingy and auto upload your run data. When you plug the USB thingy in it auto starts the ‘Nike+ Utility’, however it is started behind all other application windows. Further, on first using it it displays a login facility pre-populated with the username of ‘Guest’. Clicking in the username textbox to try and change it results in nothing ?? Nowhere does it mention it, but I have since determined that you log into the site via a browser and then the Username is picked up for the ‘Nike+ Utility’ from a cookie or something…  
What part of “we’ll show a username that the user knows is not theirs and give them no visible means of changing it” seemed sensible at the design stage ??

‘Design Horror’ 6 (DH6) falls into the areas of calibration and uploading runs. You can see from the screen shot that there is a calibration tab (the sensor is basically a pedometer and it needs to be calibrated to your stride length for accuracy) – the problem is that as soon as the USB thingy is plugged in the Nike+ utility starts and uploads any outstanding runs, ***before you calibrate***. So my very first experience with this, when I’m still in the ‘happy’ zone about my purchase is a run being uploaded that is the wrong distance. All the shiny graphics depict that I am considerably slower and run shorter distances than I actually do – what a deflation. of course, my first though is that I’ll just edit the run and update it with the correct distance  – leading me nicely to DH7.

‘Design Horror’ 7 (DH7) is the fact that I cannot edit any of the runs I upload, neither can I manually upload/input runs. With all the (fun) challenges available on the site, design to further motivate people, not providing this feature is a big letdown. Just in terms of personal motivation, Nike+ shows my Total for this year as 18 odd miles, but I didn’t buy it till July, being able to correct that with accurate values should be possible, unfortunately it’s not. Likewise some kind of import facility for all my old runs, should be – but isn’t…

‘Design Horror’ 8 (DH8) is the communication between the wristband and the Nike+ web site – it seems to be one way, certainly none of the totals from the web site are reflected in the wristband figures – I ‘reset’ my wristband the other day to recalibrate (the distances suggested by the unit were 1.05 miles in 8 out, even after calibration) and now it tells me my total is 1.93 miles (even though I calibrated it to 2 miles exactly).[![nikeplus_run](nikeplus_run_thumb.png)](https://kapie.com/content/binary/WindowsLiveWriter/NikePlusGetsAnFMinus_142A5/nikeplus_run.png)

The final ‘Design Horror’ (DH9) is the accuracy of the data. The run depicted in the image was an 8 miler I did, steady pace, no walking, flat terrain and a fast finish. the data does not reflect that, it looks like I actually stopped around 2.5 miles and slowed at the end. Nice graphs are great, but if I don’t trust your data then what’s the point.

The whole web site is flash/shockwave based and pretty slow, it is also difficult to navigate and non intuitive.

On the flip side, it is a great idea, some of the web site features are really neat – the ability to challenge other runners, join ‘virtual teams’ and have team challenges – it’s a pretty good social networking for runners site all in all. It only costs £40 for the kit (sensor and wristband), so it doesn’t break the bank.  
BUT – It could be incredible if the hardware and software are sorted out !!

***My message to Nike – Just Do It***

GEO 51.4043197631836:\-1.28760504722595