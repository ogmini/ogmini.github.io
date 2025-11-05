---
layout: post
title: Android Forensics - DJI Fly
author: 'ogmini'
tags:
 - Android
 - DJI
 - Drones
---

Recently picked up a cheap DJI Neo to play around. I have to say they're pretty cheap and easy to use. I installed the DJI Fly software on my "research" Pixel 7 because there is no way I'm installing that app on my real phone. Might as well dig into its artifacts! I imagine this has already been done and written up. I haven't looked heavily yet for prior research. Either way, it is good practice to go artifact hunting with minimal prior information. What follows are just some quick notes as I poke around.

Again, the Pixel 7 is rooted so I'm just using ADB to pull the relevant data folder. I'm looking at DJI Fly v1.18.2(1010-official) and the Pixel 7 is on Android 16 Build BP3A.250905.014.

The data folder path for the DJI Fly app is `\data\data\dji.go.v5`

I'm running the following command to generate the tar

~~~ bash
su
tar -cvf /storage/self/primary/Download/dji_fly.tar /data/data/dji.go.v5/ 
~~~

Followed by adb pull to download the tar

~~~ cmd
adb pull /storage/self/primary/Download/dji_fly.tar
~~~

I really need to script this out... I should be able to do this with:

~~~ cmd
adb shell su -c "tar -cvf /storage/self/primary/Download/dji_fly.tar /data/data/dji.go.v5/"
~~~

## Notes

There is a database folder with a bunch of (surprise) databases! I've poked my head into  `appdb.db` which is a SQLite database that appears to contain the serial number/unique identifier of associated drones. Some of the other databases are empty for me. It is very likely they are to support the fancier drones or features that I haven't play around with. Just going off names it would point towards geofencing, licensing, and the like.

There is a shared_prefs folder with a bunch of XML files which appear to be encrypted.

There is an `analytics.db` which is a SQLite database that appears to contain various log messages stored as a binary BLOB with timestamps. This is located in the files folder.
