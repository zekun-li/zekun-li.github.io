---
title: "Dual Monitors with Ubuntu 16.04 LTS"
date: 2021-01-05
description: Dual Monitors with Ubuntu 16.04 LTS
menu:
  sidebar:
    name: Dual Monitors with Ubuntu 16.04 LTS
    identifier: dual-monitor
    weight: 8
tags: ["linux", "hardware"]
categories: ["hardware"]
---

Ubuntu 16.04 default dual monitor setting sometimes doesn't work if you have one monitor placed in vertical position. In this case, the easiest way would be configure it manually.


If your second monitor is still black after plugged in the HMDI cable, remember to check the Display setting, the second monitor should be turned "On".


First you should download the CompizConfig Settings Manager.  The installation guide can be found [here](https://www.howtogeek.com/101006/how-to-tweak-unity-on-ubuntu-with-the-compizconfig-settings-manager/).


Open CompizConfig after installation. Go to General -> General Options -> Display Settings. The output field is what we should change in order to make it work.


Since we have two monitors, there should be two lines in the output field. The first line is for the monitor on the left-hand side and the second line for the one on the right. The content in each line is formatted as: WxH+X+Y where X,Y,W,H are all integer pixel values. 


Imagine a 2D coordinate system where the origin is at the top left corner, x goes to the right and y goes downward. W and H are the width and height of the monitor. X is the shift from origin to the top-left corner of the specific monitor in x direction. Similarly Y is the shift from origin to the top-left corner in y direction.  


Suppose the resolution of the first monitor is 1920x1080 and the second monitor is 1440x2560. The the first line should be 1920x1080+0+1480, and the second line should be 1440x2560+1920+0.

{{< figure src="/posts/dual-monitor/image.webp" height="400" width="600" align="center" >}}

&nbsp;

<mark> FYI </mark>, the version information of my computer is as follows:

```
lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.2 LTS
Release:	16.04
Codename:	xenial
```
