---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "ICONic green bubbles"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## DAdataTA

Title: ICONic green bubbles        
Description: What is the hex code for the Profile Picture with the number (802)495-9063

This challenge was under the Android section and worth 50 points making it the most difficult challenge. 

### My Process

- We have a phone number, so this must reference phone contacts and their ICON
    - Check databases for the phone related to contacts or address book and installed social media apps
- The hint seems obvious in that it is pointing at the Icon that people have for their profile pictures or the default colored circle. 
- Give up and move on
    
### Actual Solution

With some more time after the end of the contest, I did some more thorough poking around aLEAPP and found the "App Icons" section. Initially, I just thought this was a database of the actual icons for any installed apps. I did not realize that it contained more than just that. The "App Icons" section displays information from the com.google.android.apps.nexuslauncher\databases\app_icons.db and for the com.google.android.apps.messaging application it displays the default icon and icons for contacts! Hovering over the icons reveals the phone number and shoving the image into any photo editor can grab the color hex code of ee675c. 

### Lessons Learned

1. I need to get more familiar with aLEAPP, iLEAPP, and similar tools. This will help me understand the artifacts, their locations, and significance. I totally overlooked the "App Icons" section due to not fully understanding its relevance. 
