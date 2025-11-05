---
layout: post
title: Forensic ADB Scripts - adb-pull-tar.py 
author: 'ogmini'
tags:
 - Android
 - Python
 - ADB-Scripts
---

Coming off yesterday's post, I wrote up a quick Python script to generate and pull a tar file from a rooted Android phone. Nothing fancy, but it helps speed up my research/testing process. You can pass it a path on the Android phone and it will tar up the folder. Example would be passing it `/data/data/dji.go.v5`. You can also pass it an output path to save the tar file on to secondary storage or somewhere on internal storage. This adds a second script to my "collection" on [https://github.com/ogmini/Forensic-ADB-Scripts](https://github.com/ogmini/Forensic-ADB-Scripts).

I've also added a utility library of sorts to pull out often used functions. One big change I have incorporated into both existing scripts is support for ADB Connect over WiFi. I don't know of any new phones that support removable SD Cards. I wanted to be able to plugin in a USB storage device and use ADB at the same time. This would let me generate tar files and save them to the external storage instead of the internal storage on the phone.

The script will connect over USB initially and pull the IP of the Android phone. It will connect to that IP and perform commands. When it is done it will disconnect. You can see it in action below.

![Example](/images/adbscripts/adb_wifi.png)

I'm still working on this but it is at a minimum viable state at the moment. The option to just pass it the IP instead of needing to physically connect it would be useful. I could ping the IP just verify it is somewhat valid.

Suggestions and critiques are always welcome!
