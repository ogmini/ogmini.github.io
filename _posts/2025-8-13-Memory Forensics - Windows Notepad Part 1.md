---
layout: post
title: Memory Forensics - Windows Notepad Part 1
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

A few months back, I had a [post](https://ogmini.github.io/2025/05/13/Volatility-Windows-11-24H2-Memory-Dump-Issues.html) on using Volatility 3 to look at Windows Notepad. I had encountered issues with the symbol table and one thing that I had neglected/ignored was that the memory dump was acquired from a Hyper-V machine. I finally found some time to do some testing on a physical machine running Windows 11 23H2 and everything works as expected. I'm hoping to get a moment to test on a Windows 11 24H2 computer.

I opened Windows Notepad and created 4 tabs:

- Pre-existing unsaved tab with new changes
- Opened text file with no changes
- Opened text file with changes
- New unsaved tab with changes

I acquired a memory dump using FTK Imager.

## Volatility 3

I start with pslist to get the Process ID of notepad.exe.

~~~ cmd
vol -f "memdump.mem" windows.pslist
~~~

Using the Process ID of 2272 in my case, I next run handles to see what the process is working with.

~~~ cmd
vol -f "memdump.mem" windows.handles --pid 2272
~~~

At the moment, I'm looking for anything that looks familiar or interesting and I find our old friends the TabState/WindowState files. Interestingly, I do not find four of them as I would expect from having four tabs open. There are the two WindowState files as expected.

| Tab | TabState File Exists |Handle Exists|
| --- | --- | --- |
| Pre-existing unsaved tab with new changes | x | x |
| Opened text file with no changes | x | |
| Opened text file with changes | x | x |
| New unsaved tab with changes | x | x |

The difference between them all is that the Tab with no changes does not have a TabState file that appears as a handle. Which makes sense, I haven't made any changes to the Tab so why bother accessing the TabState file.

Next, I dump the associated files.

~~~ cmd
vol -f "memdump.mem" windows.dumpfiles --pid 2272
~~~

I could be more specific and dump the specific files by passing the virtaddr. But, might as well grab everything so I can poke at it later. Loading the three dumped TabState files in 010 Editor confirms expectations and that the "Opened text file with no changes" does not have an associated handle.

The next natural question is, where does that exist in memory? Does it? I know that the tab exists since we can see it referenced in the WindowState files.

## MemProcFS

We can duplicate the above exploration using MemProcFS. After mounting the memory dump, I browse over to the "names" folder and look for the notepad.exe process. After that, I go to the "handles" folder which is located in the "files" folder. Again, we find our TabState/WindowState files that match with what Volatility 3 dumped.
