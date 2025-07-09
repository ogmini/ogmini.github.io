---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "The SPIRITs are among us"
author: 'ogmini'
tags:
 - CTF
 - Challenges
 - Writeups
---

I'm going to flip the script for the typical writeups you see for CTFs and talk specifically about my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge.

## The SPIRITs are among us

Title: The SPIRITs are among us
Description: What link redirects you to apply for a job?

This challenge was under the iOS section and worth 75 points makng it one of the hardest in the iOS section. The two challenges worth 75 points were the only ones I was unable to solve in the iOS section.

### My Process

- Search for any obvious links in chats/emails/documents
  - Considering how many points this was worth, this seemed to easy to be a solution. The link must be obfuscated, deleted, or something.
- SPIRIT is bolded and should be a hint...
  - Snapchat is installed on the iPhone and it has a ghost as an icon! Waste all my time looking at Snapchat, ripping apart databases and other files related to it. Maybe the message was deleted and I need to recover it somehow.

### Actual Solution

There was a photo on the iPhone of the front of the SPIRIT Halloween store which had a QR Code linking to a job application website. Finding it is actually quite simple as it is in the standard location where iPhones stored photos.

- /private/var/mobile/Media/DCIM/*
- /private/var/mobile/Media/PhotoData/Photos.sqlite
- /private/var/mobile/Media/PhotoData/CPL/*
- /private/var/mobile/Media/PhotoData/Mutations/*
- /private/var/mobile/Media/PhotoData/Thumbnails/*

[https://blog.digital-forensics.it/2024/09/a-first-look-at-ios-18.html](https://blog.digital-forensics.it/2024/09/a-first-look-at-ios-18.html)

### Lessons Learned

1. I need to remember to extract all media from devices that are in standard locations. Having a listing of these locations would be one option. Another would be to use Magnet AXIOM and use the various filters in Media Explorer more intelligently. This isn't foolproof but would grab obvious media files.
2. Don't get fixated! I got hyper fixated on the Snapchat icon fitting the hint of SPIRIT. With hindsight, that connection makes no sense as it is a Ghost not a Spirit.
3. Have a healthy trust of my knowledge/skills. I didn't trust that I had looked at Snapchat from all angles. So I kept digging and digging with no results saying to myself that I must have done this wrong.
