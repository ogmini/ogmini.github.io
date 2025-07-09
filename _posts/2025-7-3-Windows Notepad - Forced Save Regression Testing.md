---
layout: post
title: Windows Notepad - Forced Save Regression Testing
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

I tried to regression test Windows Notepad 11.2410.20.0 in relation to the replication steps from [yesterday](https://ogmini.github.io/2025/07/02/Windows-Notepad-Forced-Save-on-Detecting-Manipulation.html) to see if I could get it to prompt to save.

I was unsuccesful using the same exact steps. I'm going to guess it is due to timing/race conditions. Markdown textual changes require more updates to the binary file so maybe it gives me a bigger window.

I tried writing a simple C# application to attempt to lock the TabState file. Unfortunately, C# tries to protect you and you can't really "steal" exclusive file lock from an existing process. Might have to figure out something lower level in C or find an existing tool.

I was able to write a simple C# application that shares ReadWrite access using `FileShare.ReadWrite`. If I run this application while Windows Notepad is open and make changes it has no impact. If I have it running and close Windows Notepad, it will result in the creation of the .tmp file. The .tmp will be deleted the next time that Windows Notepad is open. I tried manipulating and changing the information in the .tmp file but that seemed to have no impact. More testing is required as I only really tried for 5 minutes. Hoping I can break something or cause unintended behavior.
