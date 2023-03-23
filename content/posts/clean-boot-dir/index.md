---
title: "Clean up boot directory when full"
date: 2020-01-30
description: Clean up boot directory when full
menu:
  sidebar:
    name: Clean up boot directory when full
    identifier: clean-boot
    weight: 4
tags: ["linux", "command"]
categories: ["linux"]
---

When `/boot` directory is full, errors will occur if we try to install new packages or libraries. Here is a short blog showing how to clean up `/boot` space by getting rid of outdated kernel files).

#### Identify the current kernel version

Use the command below

```
uname -r
```

For me it’s showing up `4.4.25-040425-generic`

List all the files and folders in `/boot` directory

```
 ls /boot/
```

Then you will see lots of old kernels. The command below can list all the out-dated kernels and pipe it into purge command to uninstall.

```
 dpkg -l linux-{image,headers}-"[0-9]*" | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"` | grep -e '[0-9]' | xargs sudo apt-get -y purge
```

After the execution, the /boot directory should only contain the current kernel.