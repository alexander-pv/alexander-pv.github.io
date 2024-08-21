---
layout: page
title: Old router back to life
description: Connecting to Zyxel Keenetic 4G via ssh
img: /assets/img/projects/hardware/zyxel/zyxel-6.png
importance: 1
category: hardware
related_publications: false
---


So, I have an old but reliable router `Zyxel Keenetic 4G Rev.A`.

This is how it looks like:

Outside:
![top view](/assets/img/projects/hardware/zyxel/zyxel-1.jpg)

Inside: 
![top view inside](/assets/img/projects/hardware/zyxel/zyxel-2.jpg)

![top view inside](/assets/img/projects/hardware/zyxel/zyxel-2.jpg)

![bottom view inside](/assets/img/projects/hardware/zyxel/zyxel-3.jpg)

It seems like manufacturer added 2 antenna connectors, but something didn't go according to plan.
![bottom view inside](/assets/img/projects/hardware/zyxel/zyxel-4.jpg)


My question was: **_How can I turn it into tiny general-purpose computing device with network connection?_**
Long story short, I found out that many years ago people did that to almost any similar device before [openwrt](https://openwrt.org/)
including with the type I have.
To honor the person who figured out how to put all the necessary components for user access on a flash drive, I posted this long-lost project in Github.

[Link to the Github repo](https://github.com/alexander-pv/zyxel_4g_firmware)


The router chip has [`MIPS`](https://en.wikipedia.org/wiki/MIPS_architecture) architecture and even **20Mb** of free RAM.
![bottom view inside](/assets/img/projects/hardware/zyxel/zyxel-5.png)
![bottom view inside](/assets/img/projects/hardware/zyxel/zyxel-6.png)

Going back to the initial question, what project should I pick with requirements <20Mb of RAM and Gigabytes of flash memory 🤔


