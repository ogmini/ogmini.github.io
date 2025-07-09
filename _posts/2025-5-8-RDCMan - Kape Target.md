---
layout: post
title: RDCMan - Kape Target
author: 'ogmini'
tags:
 - DFIR
 - RDCMan
---

Just submitted a Target to KapeFiles for RDCMan. It will grab:

- `*.rdg` and `*.rdg.old` files that it finds on the drive
- RDCMan.settings file stored at `C:\Users\%user%\AppData\Local\Microsoft\Remote Desktop Connection Manager\`
- Personal Certificates stored at `C:\Users%user%\AppData\Roaming\Microsoft\SystemCertificates\My\Certificates`

The target is handling the Personal Certificates in a rather blind way as I don't believe Kape has any functionality to dynamically choose to run a target. Ideally, I'd want to check the RDCMan.settings file for the existence of the Certificate before grabbing it. This is the safest way and someone can manually check the files for relevance.

While examining one of the computers I actually use with RDCMan, I came across a RDCMan.settings.new file. I am unsure how or why it is created. I haven't done much testing on this part and so I have not added this to the Target yet.
