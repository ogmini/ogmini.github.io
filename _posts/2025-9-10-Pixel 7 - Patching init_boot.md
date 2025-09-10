---
layout: post
title: Pixel 7 - Patching init_boot
author: 'ogmini'
tags:
 - Android
 - Root
---

Previously, we unlocked the bootloader on the Pixel 7 - [https://ogmini.github.io/2025/09/09/Pixel-7-Unlocking-Bootloader.html](https://ogmini.github.io/2025/09/09/Pixel-7-Unlocking-Bootloader.html). What follows are some of my notes on the process to patch init_boot using PixelFlasher/Magisk. This will give us root on the phone and allow us to take full backups for forensic analysis.

- [PixelFlasher](https://github.com/badabing2005/PixelFlasher)
- [Magisk](https://github.com/topjohnwu/Magisk)

1. Download the [Factory Image](https://developers.google.com/android/images) from Google for the specific pixel device. You should be downloading zip file that matches the current Build Number on the device.
2. Download and run PixelFlasher
3. Point PixelFlash to your installation of Android Platform Tools. Browse to the location of the Android Platform Tools.
4. Connect phone to the computer and click "Scan". Make sure it finds your device.
5. Browse to the location of the downloaded Factory Image zip for the Device Image.
6. Click on "Rooting App" and "Download" the rooting app of your choice. In my case, I went with Magisk Stable.
7. Click "Process" for the Device Image.
8. Check the "Show All boot/init_boot" checkbox.
9. Select the relevant package and click "Patch". This will extract the init_boot and patch it in preparation of putting it back on the phone.
10. Select the new patched entry.
11. Choose "Dry Run" and click "Flash Boot". This will just do a dry run test and the phone will reboot.
12. Choose "Keep Data" and click 'Flash Boot". Again, the phone will reboot.
13. On the phone, go to the applications and you should see Magisk installed. Open Magisk to finish the installation by following the prompts. The phone will reboot again.
14. Root can be verified a few different ways. I found the easiest was to reconnect the phone to PixelFlash, click "ADB Shell", and type in `su` followed by `whoami`. It should state root.

![PixelFlasher UI](/images/androidroot/pixelflasher.png)

## Notes

- Pixel 7 / Android 16 / Build BP3A.250905.014
- Android Platform Tools 36.0.0
- PixelFlasher v8.5.1.0
- Magisk 29.0
