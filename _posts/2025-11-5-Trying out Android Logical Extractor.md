---
layout: post
title: Trying out ALEX - Android Logical Extractor
author: 'ogmini'
tags:
 - Android
 - Python
 - ALEX
---

Trying out Android Logical Extractor (ALEX) by Christian Peter today — available at [https://github.com/prosch88/ALEX](https://github.com/prosch88/ALEX). My test setup:

- Windows 11 23H2 (I need to update this testing system)
- Pixel 6A (no root) (Future Test)
- Pixel 7 (root)
- Python 3.10.11 (ALEX requires 3.11)

What follows are just some quick observations/thoughts. This is not meant to be a full review.

## Installation Steps

Pretty straightforward and this assumes you already have Python installed on the system. These steps are for Windows.

1. git clone the repository or download the zip file.
2. `pip install -r requirements.txt`
3. `winget install --id Google.PlatformTools`
4. Connect device to computer and run `adb devices` to verify
5. `python.exe alex.py`

## Options / UI

![Main Menu](/images/ALEX/main-screen.png)

On the main menu you have the following option:

- Reporting Options
  - Save Device Info
  - Create PDF Report
- Acquisition Options
  - Pull "sdcard"
  - ADB Backup
  - Logical+ Backup (UFED Style)
  - Partially Restored Filesystem Backup
- Logging Options
  - Logcat (Dump)
  - Dumpsys
  - Bugreport
- Advanced Optiosn
  - Take screenshots
  - Chat Capture
  - Query Content Providers

I tried out all of the options except for "Chat Capture" and encountered no issues/errors. I have no chats on my test Pixel 7 at the moment. I have not had a chance to run this on the Pixel 6A yet. 

ALEX is Very easy to use and gives similar vibes to [Android Triage](https://github.com/RealityNet/android_triage). I have to find some more time to dig in deeper. Feeding the output in ALEAPP and examining any differences between ALEX's capabilities on a rooted and non-rooted phone.
