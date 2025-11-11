---
layout: post
title: Examining Mobile Hotspots - Orbic Speed RC400L - Part 2
author: 'ogmini'
tags:
 - Mobile-Hotspot
 - ADB-Scripts
 - RC400L
---

Continuing to examine the Orbic Speed RC400L for any interesting digital artifacts from last [post](https://ogmini.github.io/2025/11/08/Examining-Orbit-Speed-Mobile-Hotspot.html). I'm interested in finding digital artifacts that can help place the hotspot in a place and time and also devices that connect to the hotspot. Possibly seeing if/how it logs connections to cell towers.

Today, I found two files of note:

- `/usrdata/data/usr/wlan/wlan_conf_6174.xml`
- `/data/adb_devid`

You might already be able to guess that the `adb_devid` file contains the deviceid for the hotspot which matches the one reported by the command `adb devices`.

The `wlan_conf_6174.xml` file is a little more interesting and shows the wireless access point configuration options. The RC400L supports both 2.4GHz and 5GHz wifi. Within the file you can see things such as:

- ssid
- psk (Wireless Password)
- state (You can disable)

I need to play around with this some more by changing wifi settings. In short, this XML should be holding all the configuration options for the 2.4GHz and 5GHz access points.
