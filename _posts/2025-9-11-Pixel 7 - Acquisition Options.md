---
layout: post
title: Pixel 7 - Data Acquisition
author: 'ogmini'
tags:
 - Android
 - Root
---

Currently on a train heading somewhere fun and I will be disconnected from everything. It is important in this day and age to decompress and go touch grass. There will be no posts after this one until I get back.

I spent some time reading up on options to acquire forensically sound images or backups from Android devices. I do not have access to any of the commercial options such as:

- [https://www.magnetforensics.com/resources/magnet-acquire/](https://www.magnetforensics.com/resources/magnet-acquire/) - I gather I could request access; but I don't qualify.
- [https://belkasoft.com/android](https://belkasoft.com/android)

There are free/open source options such as:

- Android Triage
  - [https://github.com/RealityNet/android_triage](https://github.com/RealityNet/android_triage)
  - [https://dfir.science/2021/10/Android-logical-acquisition-with-android_triage.html](https://dfir.science/2021/10/Android-logical-acquisition-with-android_triage.html)
- netcat/dd
  - [https://dfir.science/2017/04/Imaging-Android-with-root-netcat-and-dd.html](https://dfir.science/2017/04/Imaging-Android-with-root-netcat-and-dd.html)
  - [https://andreafortuna.org/2018/12/03/android-forensics-imaging-android-file-system-using-adb-and-dd/](https://andreafortuna.org/2018/12/03/android-forensics-imaging-android-file-system-using-adb-and-dd/)
- andriller
  - [https://github.com/den4uk/andriller](https://github.com/den4uk/andriller)

I plan on giving these a shot when I get back at some point starting with netcat/dd followed by using Android Triage.
