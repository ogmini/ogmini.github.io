---
layout: post
title: Memory Forensics - Windows Notepad Part 3
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

Making more progress while reinforcing what I've learned in the past. What we saw yesterday was an MFT entry as evidenced by the "FILE0" at the start. Just to recap my goal for this research:

> After capturing a memory dump from a live system with an active notepad.exe process. I want to recover/recreate the state of Windows Notepad as much as possible. Automate the process in some fashion.

I'm happy to say that from preliminary examination, I have done this manually on a test memory dump. General steps are:

1. Determine PID of notepad.exe process. It is possible to have multiple notepad.exe processes and this process will need to be done for all of them.
2. Find any handles related to the TabState/WindowState files.
3. Dump the TabState/WindowState files
4. Examine the WindowState files and compare to dumped TabState files. This identifies any missing TabState files and their GUIDs.
5. Dump the memory pages of the notepad.exe process.
6. Search the MFT entries for any with a filename matching the GUID from before.
7. Recover/examine the missing TabState file for filename/path
8. Search the MFT entries again for that file.
9. Profit

Screenshot of the MFT entry for a missing TabState file. You can see at the bottom the path to the original file.
![Plugin](/images/windowsnotepad/memory_missingtabstate.png)

## Next Steps

Automate this somehow. I also need to test with a larger text file to see how the MFT entries react.
