---
layout: post
title: Magnet Virtual Summit 2025 CTF - Pre-Analysis
author: 'ogmini'
tags:
 - CTF 
 - Challenges
---

I gather during CTFs you should be operating under tight OPSEC in order to win. I'm not though! So what follows is a quick writeup of my pre-analysis of the provided images. 

## Primary Individuals
There are two primary individuals of interest who have been communicating with each other and each own two of the devices. 

### Ruth Seleznyovq
Utilizes the email address/account of ruthonthego98@gmail.com. Going to assume they were born in 1998... I doubt that is important.

Owns the following devices:
| Device | Details |
| --- | --- |
| Chromebook | ChromeOS Version 129.0.6668.112 / HP Chromebook of some flavor |
| iPhone | iOS 18.0 / iPhone 14 Plus / IMSI 310280155382955 |

Google Takeout Zips:
- 20241129T224833 - was provided as part of evidence

### Mary Jones
Utilizes the email address/account of chickadeemary2000@gmail.com. Also going to assume they were born in 2000.

Owns the following devices:
| Device | Details |
| --- | --- |
| Android | Android 13 / Pixel 7a / IMEI 354977434383601 |
| Windows | Windows 11 24H2 / Dell System of some type |

Google Takeout Zips:
- 20241024T223619 - was found on the Windows System
- 20241129T224833 - was provided as part of evidence

## Story / Narrative

From a very shallow analysis of web artifacts, emails, and other sources, I can piece together a timeline of events that should hopefully help in the CTF. It is always nice to have a story and to get into the head of your suspects. 

It appears that Ruth and Mary have been in contact discussing Ruth applying or looking for a new job with Mary. They communicated by various means including email, SMS, and Discord. They met in person on 11/11/2024 around 4:20PM. One of Ruth's important folders on her Chromebook was encrypted into a 7z file by zarchiver at 11/11/2024 4:25PM. Ruth later received an email from hackergotyou@proton.me demanding ransom via BitCoin at 11/12/2024 7:57PM. At the same time, on Mary's Windows System we see web artifacts showing a login to proton.me for the hackgotyou@proton.me address around the same time as the email being received by Ruth.

### Interesting Notes

Mary has Kali Linux installed as WSL. They used sdelete on a sucess.txt.txt file. There is also a GC265HN.gpx file I haven't had a chance to examine.

They looked at a crow.jpg file and "import random.py" file. They used Windows Notepad! Hmmmm, tabstate? 



