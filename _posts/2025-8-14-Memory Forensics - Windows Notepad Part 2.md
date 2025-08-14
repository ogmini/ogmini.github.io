---
layout: post
title: Memory Forensics - Windows Notepad Part 2
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

Yesterday I left off with the mystery of the missing TabState file and today I had a little time to dump the physical memory pages that are mapped to the notepad.exe process. To reduce the number of variables in the memory capture, I cleared out all the state and *.dat files associated with Windows Notepad and opened a test text file. The text file had the following contents:

>> supercalifragilisticexpialidocious file saved

I'm not going to retread the steps I took in [Part 1](https://ogmini.github.io/2025/08/13/Memory-Forensics-Windows-Notepad-Part-1.html).

In order to dump the memory pages of notepad.exe I used the memmap plugin.

~~~ cmd
vol -f "memdump.mem" windows.memmap --pid 5632 --dump 
~~~

Popping the resulting *.dmp file into 010 Editor and searching for our text gets a hit. Below is a screenshot of the relevant section.

![Plugin](/images/windowsnotepad/mem_savedfile.png)

Making progress.

## Next Steps

Next steps are to make sense of and understand that section. I can definitely see the contents of the file and what appears to be the filename and the short filename version [https://en.wikipedia.org/wiki/8.3_filename](https://en.wikipedia.org/wiki/8.3_filename).  
