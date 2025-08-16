---
layout: post
title: Memory Forensics - Windows Notepad Part 4
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

I spent a little time today attempting to recreate my work in Volatlity 3 using MemProcFS. Helpful to see gaps/differences or possibly to see if I'm using a tool incorrectly. Initially, I found the absence of the settings.dat and Helium registry files in Volatility 3 to be a little odd. I figured that they would show up in the handles or filescan plugin. It is possible that filescan would find them; but I was getting an error about charmap and haven't had the chance to troubleshoot.

Cut over to MemProcFS and the forensic mode actually recovers the settings.dat and Helium registry files. I check the handles.csv file just to understand where/how it found them and find that the SYSTEM Process (PID 4) has handles for all of them. I promptly go back to Volatility 3 and just double check that I didn't miss those handles under PID 4 and I did not.

![Plugin](/images/windowsnotepad/memprocfs-handles.png)

I'm now left with two questions that maybe someone could provide some insight into:

1. Am I doing something wrong in Volatility 3?
2. Why the difference in behavior between Volatility 3 and MemProcFS?

Mark McKinnon kindly left a comment on yesterday's post about using Autopsy with the MemProcFS plugin to automate. I could tie that with my existing WindowsNotepadParser to parse the state files, an existing plugin to parse the Helium Registry Files, and the in progress RECmd/Registry Explorer [plugin](https://github.com/EricZimmerman/RegistryPlugins/pull/68#issuecomment-3172819694) for ApplicationSettingsContainer to read the settings.dat.

Just a reminder that from [Part 1](https://ogmini.github.io/2025/08/13/Memory-Forensics-Windows-Notepad-Part-1.html), neither Volatlity 3 or MemProcFS found the TabState file for the opened text file with no changes. No handle existed. We found in later posts that it is possible to recover it manually.

I still want to persist in writing a Volatility 3 plugin for the experience; but the above feels like it might be faster/easier for a first pass.
