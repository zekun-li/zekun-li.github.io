---
title: "Raspberry Pi Connect To WIFI with Command"
date: 2022-10-14
description: Connect Raspberry Pi to WIFI without GUI
menu:
  sidebar:
    name: Raspberry Pi Connect To WIFI with Command 
    identifier: raspberry pi wifi
    weight: 11
tags: ["linux", "raspberrypi","command"]
categories: ["raspberrypi"]
---

If you have an extra monitor, keyboard and mouse, then connecting Rapsberry Pi to WIFI is really easy. You just click a few buttons in the GUI and you're done. However, these gadgets are not always handy. In this case, you will need to connect the Pi with WIFI with command line. This blog shows you how to do it. 
 
##### Things to prepare:

* Ethernet Cable  
* A device with network connectivity (mine is MacBook)

##### Steps

1. On the WIFI-connected device, type `ifconfig | grep broadcast | arp -a`. Devices in LAN will show up. Sample output:
	```
	? (169.254.168.39) at e4:5f:1:4d:59:b0 on en0 [ethernet]
	? (192.168.0.1) at ec:4f:82:81:af:cc on en0 permanent [ethernet]
	? (224.0.0.251) at 1:0:5e:0:0:fb on en0 ifscope permanent [ethernet]
	? (239.255.255.250) at 1:0:5e:7f:ff:fa on en0 ifscope permanent [ethernet]
	```
2. Connect Pi to router using ethernet cable
3. Retype the above command, and a new device should show up. For me, it is 
	```
	? (192.168.0.56) at e4:5f:1:4d:59:b0 on en0 ifscope [ethernet]
	```
4. SSH to Pi with the above IP address. Example: `ssh pi@192.168.0.56`
5. After logging in, type `sudo raspi-config`, then "System Options" -> "Wireless LAN" -> "Enter SSID" -> "Enter password". <mark>Note:</mark> The SSID is the WIFI name when you click the WIFI symbol on any device. 
6. Finish and exit the config.
7. To verify the PI is connected with WLAN, do `iwgetid`, and the WLAN name should show up. 
8. Now if you do `ifconfig | grep broadcast | arp -a` , a new line should show up 
	```
	? (192.168.0.57) at e4:5f:1:4d:59:b1 on en0 ifscope [ethernet]
	```

Then you're done, this above IP address is the one to use when connecting Pi with WIFI. 
