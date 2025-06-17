---
layout: post
title: KAPE Target - Windows Notepad
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
 - KAPE
---

Finally got a few minutes to start working on writing the KAPE Target for Windows Notepad. It will grab the Tab State, Window State, and settings.dat files that I've talked about in my [research](https://github.com/ogmini/Notepad-State-Library). Pretty straightforward process and I just want to do a little more testing and make sure I have everything right before I submit a Pull Request.

I've also started on writing a KAPE Module that leverages my Windows Notepad Parser that will generate CSV files from the above KAPE Target. I'm hoping I can get this all done this weekend in between normal work.