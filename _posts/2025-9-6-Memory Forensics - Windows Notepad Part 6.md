---
layout: post
title: Memory Forensics - Windows Notepad Part 6
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

Following from Part 5, I wanted to test what happens if the TabState file is too large to natively fit into the MFT Entry. I opened Windows Notepad and typed some test text and repeatedly copy/pasted it back until it was sufficiently large. Took a memory dump and proceeded to analyze it using the same steps as outlined in [Part 5](https://ogmini.github.io/2025/08/29/Memory-Forensics-Windows-Notepad-Part-5.html).

As expected, I was unable to get the contents of the TabState file as it no longer fit. Without a disk image, I do not believe anything is retrievable purely from memory.

## Next Steps

I've been doing these tests on a physical machine that only has one single 4GB stick of DDR4 running Windows 11 24H2. As you can imagine, it is incredibly slow and painful. My reasoning was to make it faster to capture the memory dump and easier to analyze it as it would be smaller. I'm now wondering if I'm running into issues and possibly missing things as Windows manages the memory. I literally got out of memory errors during testing.  

I also admit that the above testing was done rather haphazardly and not up to standards. So, I will be methodically rerunning these tests with more memory in the physical machine.
