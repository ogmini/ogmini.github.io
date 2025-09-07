---
layout: post
title: Pixel 7 - Researching Rooting Options
author: 'ogmini'
tags:
 - Android
 - Root
---

A few weeks back, I picked up a used Pixel 7 to use as a testbed to learn Android forensics. The intention is to root the phone, start poking around, and see where it leads me. Larger goal is to start contributing to ALEAPP. I specifically picked the Pixel 7 as it is a popular phone with a bootloader that is easily unlocked. 

For now, I've been reading up on how to go about rooting the phone on the [XDA Forums](https://xdaforums.com/). I'll add the silly disclaimer that if you attempt any of this you are on your own and I bear no responsibility. It appears the general steps of rooting a Pixel phone are:

1. Install [ADB](https://developer.android.com/tools/releases/platform-tools) on a computer
2. Unlock Bootloader (Slamming that build number in Settings)
3. Download factory image [https://developers.google.com/android/images](https://developers.google.com/android/images)
4. Unzip and move `init_boot.img` to the phone
5. Use [magisk](https://github.com/topjohnwu/Magisk) to patch the file
6. Use ADB to flash the patched `init_boot.img` back to the phone
7. Reboot 

I will probably use a tool called Pixel Flasher from badabing2003 at [https://xdaforums.com/t/pixelflasher-a-gui-tool-for-flashing-updating-rooting-managing-pixel-phones.4415453/](https://xdaforums.com/t/pixelflasher-a-gui-tool-for-flashing-updating-rooting-managing-pixel-phones.4415453/). It is just a matter of getting the time and confidence to give this a shot.
