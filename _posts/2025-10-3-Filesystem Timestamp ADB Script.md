---
layout: post
title: Android Forensics - Filesystem Timestamps ADB Script 
author: 'ogmini'
tags:
 - Android
 - Timestamps
---

In my last few posts, I've been looking at the lifecycle of digital photos taken on an Android phone - [https://ogmini.github.io/tags.html#Google-Photos](https://ogmini.github.io/tags.html#Google-Photos). When researching and doing testing, it can be very useful to script as many steps as possible so that you can recreate testing quickly and consistently in different scenarios. I need to be able to easily pull the filesystem timestamps and MD5/SHA256 hashes along with the digital phones from an Android Phone. Spent a little time today throwing together a Python script to leverage various Android Debug Bridge (adb) commands.

The script is currently called script.py because I have no imagination. You pass it a folder path (Ex. `/storage/self/primary/DCIM/Camera/`) and it will identify all the files located in that location. Using md5sum, sha256sum, and the stat commands, the script captures hashes and filesystem timestamps before saving that information to a csv file. Afterwards, the script will pull all the files using adb pull.   

Some areas for improvement; but it works for my purposes for now. I'll probably add the ability to iterate through subfolders and maybe a mask for files. I will put the script on my GitHub at some point.