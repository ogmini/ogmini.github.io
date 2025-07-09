---
layout: post
title: Windows Notepad - Rewrite / AI
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
 - rewrite-AI
---

Microsoft had previously added Rewrite to Windows Notepad on their dev release channels. It is now live to the public as of Windows Notepad Version 11.2412.16.0 and requires a subscription to Microsoft 365. I've already partially updated some of my documentation to note the new Application Hive entries related to Rewrite. 

| KeyName | Type | Notes |
|---|---|---|
|RewriteEnabled|0x5f5e10b| 0 = Off / 1 = On
|RewriteTeachingtip|0x5f5e10b| 0 = Off / 1 = On

I have yet to search for any other interesting artifacts. I don't have a Microsoft 365 account (easy fix) and I've recently taken on a new role at work which has consumed a lot of time (not so easy fix). I'm interested to see how the "Rewritten" text is handled. Is it stored somehow in the Tabstate files or somewhere else? Can we see network traffic related to the Rewrite request? So many questions. 