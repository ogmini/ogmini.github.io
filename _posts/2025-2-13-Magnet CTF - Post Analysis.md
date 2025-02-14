---
layout: post
title: Magnet Virtual Summit 2025 CTF - Post-Analysis
author: 'ogmini'
tags:
 - CTF 
 - Challenges
---

![15th Place!](/images/MVS2025CTF.png)

First of all, big thanks to all the organizers who made the challenges and did all the hard work! As I said in the Discord, "Definitely learned a lot and most importantly ways to improve my workflow/process". 

 - Fatima O. 
 - Yehuda Bollen 
 - Adam Hachem 
 - Cece Ehgotz 
 - James Cangelosi 
 - Nathan Kreit 
 - Kevin Pagano
 - Jessica Hyde

 [LinkedIn Post](https://www.linkedin.com/posts/hydejessica_mvs2025ctf-dfir-activity-7295898860420321280-arbP?utm_source=share&utm_medium=member_desktop&rcm=ACoAAADfdBEBELHpzRkuF5mAh39sI_EZfTyW4lg) from Jessica Hyde to all of their LinkedIn profiles.

 I am really happy with placing 15th at the buzzer! I was able to carve out sometime to compete and I'm currently sitting 5th as I write this post. Excited to participate in the future.  

## Lessons Learned

### Notes!

I need to figure out a fast and efficient way to take notes of how I came to the solutions. I took very few notes during the competition and it will be more time consuming to do an adequate writeup. Do I just record video of my desktop? Record voice notes? I'll have to do some research.

### Expanded Pre-Analysis

I did a little bit of pre-analysis on the evidence provided. I was able to solve a bunch of questions right off the back by referring to my notes. Hilariously, I didn't note the telephone numbers of the smartphones; but I noted the IMEIs! My interest in the password protected 7zip file and other artifacts were warranted. I should have attempted to explore them more before the competition.

I should pre-dump and pre-analyze the registry.

Dump all images with EXIF data and run them through my little script [https://github.com/ogmini/EXIFtoKMZ](https://github.com/ogmini/EXIFtoKMZ) or some other similar tool. 

Honestly, there is so much pre-analysis that can be done and these were the ones that stood out. 

### Emails/Texts

I need to find a better way to be able to search and understand emails. I wonder if something like [ChatRTX](https://www.nvidia.com/en-us/ai-on-rtx/chatrtx/) from Nvidia might be useful for this. Worth investigating. For one of the questions, I hyper focused on emails and forgot to check text messages. Feeding all that to an AI model might be worth the work. I found it very tedious to actually have to READ emails and texts. I do enough of that in my actual job.

### Workstation / Lab Setup

Competing on a 14" laptop screen isn't ideal, but the circumstances demanded it. My normal computer with three screens would have been ideal. I actually do all my work on Virtual Machines and I spun one up specifically for this competition. Helps me keep everything clean, isolated, and accessible from any computer using Tailscale. In the future, I want to explore spinning up one Virtual Machine for each image/device given. I think it would have been easier/faster to flip between four different virtual machines for the iPhone, Chromebook, Pixel, and Windows System. I could just leverage [Remote Desktop Connection Manager](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman). 

## Closing Comments

I want to commend the organizers for making a dataset with a very real feeling scenario. It makes for a more compelling challenge. I have one suggestion to do validation on the submission of answers to not allow blank entries. I stupidly pressed submit on an empty box and it wasted a chance. 

I'm still trying to solve the "DAdataTA" challenge; but I have a sneaky feeling that the content was 11 characters long. This is assuming I'm reading the artifacts from Windows Notepad correctly. 

