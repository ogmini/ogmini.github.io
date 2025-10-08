---
layout: post
title: Gmail App - IMAP Account Artifacts (Attachments) - Part 3
author: 'ogmini'
tags:
 - Android
 - ALEAPP
---

I somehow missed this in yesterday's [post](https://ogmini.github.io/2025/10/06/Gmail-App-IMAP-Account-Attachments-Part-2.html) and it popped into my head on my commute to work this morning. We learned that a sent email that has attachments is stored differently in the `Attachment` table with a reference to a "cachedFile" which is a content link. In our test example we had:

`content://com.google.android.gm.email.provider/attachment/cachedFile?filePath=%2Fdata%2Fuser%2F0%2Fcom.google.android.gm%2Fcache%2F2025-10-06-15%3A49%3A463611689731535944683.attachment`

Looking more closely at the cached file that exists. It has a filename of `2025-10-06-15_49_463611689731535944683.attachment` which does not match with the actual filename. I had however, glossed over the possible significance of the format of the filename. In this case, it is pretty clearly a timestamp of some sort. Looking at the `Message` record for the sent email we can see that it has a "timeStamp" of 1759780186029. This converts to "Monday, October 6, 2025 3:49:46.029 PM" localtime for the phone which matches up to the 2025-10-06 15:49:46 that we see present in the filename. At th moment, I'm unsure if the "3611689731535944683" is part of the time or has some other significance as it doesn't appear to match up with the message "timeStamp". 
