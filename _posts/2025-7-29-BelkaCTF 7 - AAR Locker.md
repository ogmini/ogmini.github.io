---
layout: post
title: BelkaCTF 7 - AAR Locker
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task2.png)

Another fundamental task, identify the malware process from a RAM dump. The easiest, first step is to just look at the process names and see if anything looks out of place.

## Volatility 3

Running pslist will give us a list of processes.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.pslist
~~~

We can of course pipe the output to a text file or just scroll through the command window. Doing this, a process called epxlorer.exe is found with a PID of 9920 that seems very suspicious being a mispelling of explorer.exe.

![PID 9920](/images/BelkaCTF7/Task2-1.png)

Digging in deeper, we can run pstree to see where this process spawned from. Pretty suspicious and most likely our malware process.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.pstree --pid 9920
~~~

![PID 9920 pstree](/images/BelkaCTF7/Task2-2.png)

We'll save further analysis of the file for later challenges.

### MemProcFS

We go to the `name` folder of the mounted memory image and look for a process name that looks suspicious and find a folder called `epxlorer.exe-9920`.

We'll save further analysis of the file for later challenges.

## Thoughts

Hilariously, this one was very straightforward and just required looking at the process names. I was all prepared to have to use plugins such as malfind, do a deeper analysis with pstree, examine odd paths, run psscan, etc. A good reminder that not everything has to be complex or complicated. I initially ignored the process names and immediately launched into using malfind.
