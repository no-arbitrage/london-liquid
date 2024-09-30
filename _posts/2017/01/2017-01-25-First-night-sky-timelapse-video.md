---
title: "First night sky timelapse video"
date: "2017-01-25T13:46:59"
tags: [
  "blog"
]
image: 'assets/images/first-night-sky-timelapse-video-image_002.jpg'
---
So I had some (limited) success with my astrophotography last night. It was a clear night for an hour or so and I managed to get the settings for **raspistill** tweaked to capture some night stars. I grabbed a couple of hundred shots and then converted those into a short video.

The settings I used for **raspistill** were:

```
raspistill -bm -tl 1000 -v -set -ISO 800 -awb off -awbg 1,1 -t 21600000 -ss 6000000 -o %06d.jpg
```

This created a bunch of .jpg files, but skipped tons of frames, so the numbering sequence was way out of kilter (000001.jpg, 000048.jpg, 000073.jpg etc). To fix that, and get them into a proper sequence I used a little script as follows:

```
I=0 
for F in 0\*.jpg; do 
  echo "$F" \`printf image\_%06d.jpg $I\` 
  mv "$F" \`printf image\_%06d.jpg $I\` 2>/dev/null || true 
  I=$((I + 1)) 
done
```

Then, to convert the individual jpgs into an mp4 movie I used **avconv** in the **libav-tools** package as follows:

```
avconv -i image\_%03d.jpg -r 5 -vf scale=1280:720 -vcodec libx264 -r 5 star-trail-$(date "+%Y%m%d-%H%M%S").mp4
```