---
layout: post
title: Gmail App - IMAP Account Artifacts (Message Logging) - Part 1
author: 'ogmini'
tags:
 - Android
 - ALEAPP
---

Just put in the [PR](https://github.com/abrignoni/ALEAPP/pull/603) to update the ALEAPP plugin to handle attachments. Details about attachments can be found [https://ogmini.github.io/2025/10/06/Gmail-App-IMAP-Account-Attachments-Part-2.html](https://ogmini.github.io/2025/10/06/Gmail-App-IMAP-Account-Attachments-Part-2.html). Once I "finish" tearing into IMAP artifacts in the Gmail App, I will write up a summary post/report.

In the meantime, I was testing moving and deleting emails. Initially, I wanted to see what would happen to cached attachments if you deleted an email (They are also deleted). However, during testing I noticed that two tables now had entries in them.

- MessageMove
- MessageStateChange

I need to do more testing and create more test cases; but, they appear to do exactly what it says on the tin. Look for a future post about this and again an update to the ALEAPP plugin.
