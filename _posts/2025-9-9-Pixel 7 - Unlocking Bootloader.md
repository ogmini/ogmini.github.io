---
layout: post
title: Pixel 7 - Unlocking the Bootloader
author: 'ogmini'
tags:
 - Android
 - Root
---

Making progress in rooting the Pixel 7 phone that I picked up. I spent some time today just unlocking the bootloader. Pretty straightfordward process at least for a Pixel device. What follows are some of my notes on the process to unlock the bootloader.

Useful references:

- [https://grapheneos.org/install/](https://grapheneos.org/install/)
- [https://www.youtube.com/watch?v=MzQTOv0Kcvs](https://www.youtube.com/watch?v=MzQTOv0Kcvs) - For the visual among us.

1. Download/Install the [Android Platform Tools](https://developer.android.com/tools/releases/platform-tools). I just downloaded the zip and ran the tools from the extracted folder. You could also install Android Studio or update your PATH environment variable.
2. Connect phone to computer. The phone will prompt you to "Allow USB Debugging". This is an easy step to miss.
3. Update [USB Drivers](https://developer.android.com/studio/run/oem-usb#InstallingDriver). This might be optional.
4. Disconnect phone from computer.
5. Boot phone into fastboot by restarting the phone and holding down the volume down button as it boots. Should see "Fastboot Mode" on the screen among other text.
6. Connect phone to computer.
7. Open command prompt to the Android Platform Tools and run `fastboot devices`. This is just to verify the phone is connected. You should see a device listed.
8. Run `fastboot flashing unlock`. The phone's screen should change to the bootloader unlock screen.
9. Use the volume keys to choose the "Unlock the bootloader" option. This will wipe the phone as stated, restart, and leave you at the setup/activation screens.
