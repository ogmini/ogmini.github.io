---
layout: post
title: Memory Forensics - Windows Notepad Part 5
author: 'ogmini'
tags:
 - Volatility
 - Memory-Forensics
 - Windows-Notepad
 - MemProcFS
---

Let us recap what we've learned already and the process to manually recover Windows Notepad artifacts from memory. I am starting with a fresh memory dump for the scenario where we've opened a text file and made no changes. There are no other tabs. I had someone else create the scenario and memory dump so that I would not know anything about the text file that was opened.

~~~ cmd
vol.py -f memdump-savedfile.mem windows.pslist
~~~

![pslist](/images/windowsnotepad/mem-1.png)

Running pslist allows us to identify the PID of any active "Notepad.exe" processes. In our case, we focus on PID 6780.

~~~ cmd
vol.py -f memdump-savedfile.mem windows.dlllist --pid 6780
~~~

![dlllist](/images/windowsnotepad/mem-2.png)

I like to run dlllist for the PID just to verify that this is the right executable and it also provides us the version of Windows Notepad. The Path should be `C:\Program Files\WindowsApps\Microsoft.WindowsNotepad_11.[VERSION]_x64__8wekyb3d8bbwe\Notepad\Notepad.exe`.

~~~ cmd
vol.py -f memdump-savedfile.mem windows.handles --pid 6780
~~~

![handles](/images/windowsnotepad/mem-3.png)  
![handles2](/images/windowsnotepad/mem-4.png)

Running handles gives us a good idea of what files PID 6780 is holding. We can see entries for the Window State files and a Tab State file.

| Type | File | Offset |
| --- | --- | --- |
| WindowState | d72f950c-6bd9-4aaf-8cf4-bc94e8fb426c.0.bin | 0x858a0ef93800 |
| WindowState | d72f950c-6bd9-4aaf-8cf4-bc94e8fb426c.1.bin | 0x858a0ef955b0 |
| TabState | c05ecb11-6d18-478f-9807-6554828d0bf2.bin | 0x858a1f079380 |

~~~ cmd
vol.py -f memdump-savedfile.mem windows.dumpfiles --virtaddr 0x858a0ef93800
vol.py -f memdump-savedfile.mem windows.dumpfiles --virtaddr 0x858a0ef955b0
vol.py -f memdump-savedfile.mem windows.dumpfiles --virtaddr 0x858a1f079380
~~~

I attempt to dump the files; but all three fail. At the moment, I don't know enough to understand why beyond a guess that the information is no longer resident in the memory.

~~~ cmd
vol.py -f memdump-savedfile.mem windows.memmap --pid 6780 --dump
~~~

I dump the memory of PID 6780 to continue the hunt. This is where it gets more complicated. I'm sure there is a better way to do this and hopefully someone might be able to point me in a better direction. From previous steps, we know the names of the various state files and this will shortly come in handy. Taking our `pid.6780.dmp` file, I open it up in 010 Editor. Methodically, I search for the various filenames of the State files that we identified using the handles plugin and bookmark anything that things that look like MFT entries. Specifically, we're looking for `FILE0`.

> Remember, the filename is stored in UTF-16 encoding. A handy CyberChef recipe to do the conversion to Hex for you - [https://cyberchef.org/#recipe=Encode_text('UTF-16LE%20(1200)')To_Hex('Space',0)&input=ZDcyZjk1MGMtNmJkOS00YWFmLThjZjQtYmM5NGU4ZmI0MjZjLjAuYmlu](https://cyberchef.org/#recipe=Encode_text('UTF-16LE%20(1200)')To_Hex('Space',0)&input=ZDcyZjk1MGMtNmJkOS00YWFmLThjZjQtYmM5NGU4ZmI0MjZjLjAuYmlu)

![mft-1](/images/windowsnotepad/mft-1.png)  
![mft-2](/images/windowsnotepad/mft-2.png)  
![mft-3](/images/windowsnotepad/mft-3.png)

As seen in the above screenshots, MFT entries for all three State files are identified and the Window State files are usually very small and can most likely entirely fit in the MFT entry's Data portion. It is also possible that the Tab State would also fit in the MFT entry's Data portion. In our case, both are true as we can see below. I've highlighted the Magic Bytes ("NP") and the relevant hex in the screenshots below. At this point, I would encourage you to read the documentation in my [Notepad State Library](https://github.com/ogmini/Notepad-State-Library) as it will give you a better understanding of the structure and behavior of the state files. I will be glossing over details on how to read the files.

![mft-4](/images/windowsnotepad/mft-4.png)  
![mft-5](/images/windowsnotepad/mft-5.png)  
![mft-6](/images/windowsnotepad/mft-6.png)

I copy/paste the highlighted hex into new files in 010 Editor for easier analysis. This allows me to apply my Tab State and Window State templates.

![hex-1](/images/windowsnotepad/hex-1.png)  
![hex-2](/images/windowsnotepad/hex-2.png)  
![hex-3](/images/windowsnotepad/hex-3.png)

The Window State files confirm that only one tab is open with the GUID of `c05ecb11-6d18-478f-9807-6554828d0bf2`. There is the possiblity of one of the Window State files having a list of different tabs so you should always check both. Taking a look at that Tab State file confirms that this is a saved file with no changes. I also get the path and filename of the opened text file as `C:\Users\User\Desktop\MyFile.txt` which gives us our next step.

Before we move on though, I take another look at the MFT entries by copy/pasting the MFT entry hex into new files in 010 Editor for easier analysis. This allows me to apply Eric Zimmerman's MFT template. I'm able to see the various timestamps giving us more insight into when files were opened or changed. Running the MFTScan plugin for Volatility 3 also provides us the same timestamps.

~~~ cmd
vol.py -f memdump-savedfile.mem windows.mftscan.MFTScan
~~~

I go back to the dump of PID 6780 and search for the filename of `MyFile.txt` and find an MFT entry. I'm able to recover the contents (`This file that I have saved to disk with no changes`) of the text file as it was small enough to fit in the MFT entry. Again, examining the MFT entry also provides us the various timestamps for the text file which are useful for timelines.

![mft-7](/images/windowsnotepad/mft-7.png)

## Next Steps

I need to do similar testing/analysis with a text file that is too big to fit in the MFT entry. I should also be able to make Window State and Tab State files that are too big to fit in the MFT entry. For the moment, I've given up on writing a Volatility 3 plugin to automate this process as it has not been easy wrapping my head around that challenge. It is also more important to solidify and document the process.

## Recap

These notes/shorthand are mainly for me.

1. pslist - find notepad.exe processes
2. dlllist - verify this is for Windows Notepad
3. handles - find handles for State files
4. dumpfiles - attempt to dump state files
5. memmap - dump memory of process
6. manually search for MFT Entries related to state files
7. manually analyze MFT Entries
    - MFT Entries may include full contents of file if small enough
8. manually search for MFT Entries related to any resident text files
