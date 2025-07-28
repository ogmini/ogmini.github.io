---
layout: post
title: BelkaCTF 7 - Volatility 3 vs MemProcFS
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
---

I encountered an interesting situation during BelkaCTF 7. This is not a writeup for one of the challenges but will give the solution as part of examining the oddity I experienced. I'm hoping that someone might be able to shed some light on the reason for the difference or if I did something incorrectly. One of the challenges required dumping a process from memory for further analysis. What is baffling is that I'm pretty sure that Belkasoft X uses Volatility 3 for processing memory images. But using Volatility 3 manually did not work for me. I ended up getting the right answer with MemProcFS. 

## Volatility 3

Using the latest version of Volatility 3 2.26.0. I ran the following command to dump the identified process.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.pslist --pid #### --dump
~~~

## MemProcFS

Using the latest version of MemProcFS 5.15. I browsed to the location of the dumped process.

~~~
\\name\[processname].exe-[PID]\modules\[processname].exe\pefile.dll
~~~

## Differences

| Tool | Size | MD5 |
| --- | --- | --- |
| Volatility 3 | 1,138,688 bytes | ec3a90bdde9ca0cb315b8789795436f1 |
| MemProcFS | 1,111,552 bytes | 75ea94b54420c39dcd3d8ce574ba9d34 |

Comparing them in 010 Editor shows differences between the files scattered throughout. Either one of these files would have let you solve the later challenges. 