---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Another week, another David Cowen Sunday Funday challenge posted at his [blog](https://www.hecfblog.com/2025/04/daily-blog-807-sunday-funday-41325.html) and it is about looking for artifacts left behind by browser password extraction programs such as WebBrowserPassView. 

Can I defend my streak? I'm not in this to win anything. These challenges provide a good way to focus on different areas and work on methodology. I'm really hoping for others to participate as I feel I can learn a lot more from their writeups. 

## Challenge

It's becoming more common that the first thing an attacker will try to do if they get access to a user's system is extract all of the saved browser passwords. Profile a popular browser password extractor (such as WebBroweerPassView or HackBrowserData) and detail what artifacts are left behind that would reveal their usage on a Windows 11 system. Extra points if you:
a. Try multiple browser password viewing tools
b. Try MacOS as well as Windows

## Test Setup

The browser password extractor is being left up to us though we were given two examples. I will be testing for artifacts left behind by [LaZagne](https://github.com/AlessandroZ/LaZagne). I will not be doing anything fancy to obfuscate or hide the execution of the program. As always, Windows 11 24H2 will have a standard installation. If I get feisty, we'll delve into other ones such as the aforementioned [WebBrowserPassView](https://www.nirsoft.net/utils/web_browser_password.html).

1. Save some passwords into the Edge Browser
    - At this point, most of the popular browser are all Chromium based.
2. Execute `LaZagne.exe browsers -oN`
    - We may need to put Windows Defender exclusions into place.
3. Look for artifacts

### Unrealistic Steps

From my viewpoint, the following steps would be a little unrealistic for an attacker to actually perform. They would leave behind more artifacts though...

4. Open/verify the contents of the output file from LaZagne
5. Extract the file by uploading to an open NextCloud shared folder
6. Look for more artifacts

## Assumptions

Honestly, I'll be surprised if we can find any weird artifacts. I may have to add Windows Defender exclusions to let the program run. An attacker could customize and compile their own version but that will be out of scope of this testing. This could make it harder to detect. I will look for artifacts of these exclusions being put into place and later removed. 

There might be some artifacts left in the Prefetch files, Shimcache, and Amcache. I don't expect to see much in the Event Logs; but it is worth checking. 
