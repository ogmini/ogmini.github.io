---
layout: post
title: Examining Mobile Hotspots - Orbic Speed RC400L - Part 3
author: 'ogmini'
tags:
 - Mobile-Hotspot
 - ADB-Scripts
 - RC400L
---

Still looking at the Orbic Speed RC400L hotspot. You can find all the previous posts at [https://ogmini.github.io/tags.html#Mobile-Hotspot](https://ogmini.github.io/tags.html#Mobile-Hotspot). Today, I started looking at the various log files located at `/data/logs`.

There are 5 log files:

| Log File | Notes |
| --- | --- |
| atfwd | Possibly logs AT commands? |
| cpe | Possibly logging data usage? |
| fota | Checking for upgrade? Maybe Firmware OTA? |
| kn | General log file? I see entries for dnsmasq-dhcp, hostapd, and kernel stuff |
| oma | ?? |

There is a lot to unpack here and it is delving into cellular technology of which I don't know too much about. Good example are those AT commands. I have a lot of research and reading ahead of me.

## References

- [https://onomondo.com/blog/at-commands-guide-for-iot-devices/](https://onomondo.com/blog/at-commands-guide-for-iot-devices/)
