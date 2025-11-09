---
layout: post
title: Examining Mobile Hotspots - Orbic Speed RC400L
author: 'ogmini'
tags:
 - Mobile-Hotspot
 - ADB-Scripts
---

I've been following the Rayhunter project from EFF over at [https://github.com/EFForg/rayhunter](https://github.com/EFForg/rayhunter). I've had and used an Orbic Speed RC400L as a mobile hotspot for many years. I recently replaced it with a 5G compatible hotspot for the speeeeeeeeeeeeeed. I actually hate the newer one as it boots up way slower. Either way, I have an unused Orbic Speed RC400L hotspot now that is ripe for playing around with. While researching Rayhunter, I was interested in what digital artifacts could be recovered from a stock device. The project to install Rayhunter was put on hold for this.

Researching the device led me to a writuep by Matthew Garrett from 2022 at [https://mjg59.dreamwidth.org/61725.html](https://mjg59.dreamwidth.org/61725.html) where he was interested in getting it to autoboot when plugged into USB at [https://mjg59.dreamwidth.org/62419.html](https://mjg59.dreamwidth.org/62419.html). You can get unprivileged adb access to the file system. So I set about recreating some of his steps as I was only interested in getting adb access. 

I had about 30 minutes today and was able to successfully get adb acccess. I have not extensively explored the file system and I might need to slap a SIM card back in this thing to get any meaningful data. This is what I found:

![adb](/images/orbicspeed/adb.png)

I censored out the DeviceId from pure paranoia. Running `cat /proc/version` spits out `Linux version 3.18.48 (make@ub14server) (gcc version 4.9.3 (GCC) ) #1 PREEMPT Thu Oct 8 21:40:29 CST 2020`. Not sure how useful this is yet...

More to come in the future as I get more time. 

## References
- [https://xdaforums.com/t/resetting-verizon-orbic-speed-rc400l-firmware-flash.4334899/post-89993011](https://xdaforums.com/t/resetting-verizon-orbic-speed-rc400l-firmware-flash.4334899/post-89993011)
- [https://mjg59.dreamwidth.org/61725.html](https://mjg59.dreamwidth.org/61725.html)
- [https://mjg59.dreamwidth.org/62419.html](https://mjg59.dreamwidth.org/62419.html)
