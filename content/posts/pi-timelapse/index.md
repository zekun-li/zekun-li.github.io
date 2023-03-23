---
title: "Raspberry Pi Timelapse Video"
date: 2021-11-14
description: Raspberry Pi Timelapse Video
menu:
  sidebar:
    name: Raspberry Pi Timelapse Video
    identifier: server-vscode
    weight: 9
tags: ["raspberrypi", "camera","timelapse"]
categories: ["raspberrypi"]
---

Recording a timelapse video of a sunset can be a fantastic way to capture the beauty of nature in a condensed form. Without a bulky professional camera, you can still record a timelapse video with Raspberry Pi easily. In this blog post, we'll introduce the `raspistill` and `ffmepg` command for creating videos.

We can take a look at a demo video shot with pi-camera v1.3 on Raspberry Pi. It was a maganificant sunset in Minnesota. 


{{< youtube 8EsdH4ZL85w >}}

&nbsp;

#

First use `raspistill` to take a sequence of pictures during a period of time (e.g. 2 hours), then use `ffmepg` to convert the pictures to a video.
```
raspistill -n  -q 100 -ex night \
   -o pi_space/time_lapse_night/img_%05d.jpg -mm matrix -drc low \
   -t $((2*3600*1000)) -tl 3000

```
- `-n` do not display preview window (use this when you don't have graphical interface to Raspberry Pi)
- `-q` set the image quality, range (0,100)
- `-ex` set the exposure mode. It's the best to use `night` when the light is dim
- `-o` path to the output file
- `-mm` set metering mode, options: [`average`,`spot`,`backlit`,`matrix`]
- `-drc` set DRC(dynamic range control) to boost the image visibiltiy with different lighting condition. `low` for dim light
- `-t` set recording period (unit: ms)
- `-tl` set time lapse between two frames （unit: ms)


```
ffmpeg -framerate 30 -i time_lapse_night/img_%*.jpg -c:v libx264 -profile:v high -crf 20 -pix_fmt yuv420p time_lapse_night/a_output.mp4
```

Usually the `-framerate` ranges from 25 to 60. `libx264` encoding generates video in mp4 format. 

