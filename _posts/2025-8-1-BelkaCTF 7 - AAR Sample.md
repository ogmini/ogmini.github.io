---
layout: post
title: BelkaCTF 7 - AAR Sample
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task3.png)

Sounds so simple, just get the MD5 hash of the malware running on the machine. I learned something new about the behaviour of Volatility 3 and the windows.dumpfiles plugin. I had made reference to this in a previous post and now I have some of the answer - [https://ogmini.github.io/2025/07/27/BelkaCTF-7-Volatility3-vs-MemProcFS.html](https://ogmini.github.io/2025/07/27/BelkaCTF-7-Volatility3-vs-MemProcFS.html). A lot of this writeup is a rehash of that post just with the details filled in.

## Volatility 3

We know from the [Locker](https://ogmini.github.io/2025/07/29/BelkaCTF-7-AAR-Locker.html) task that the PID of our malware is 9920.

We can dump the malware by using the dumpfiles plugin.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.dumpfiles --pid 9920
~~~

There is another option where we run the filescan plugin to get the offset of the malware.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.filescan
~~~

This gives us an offset of 0xe6892af1f1f0 for `\Users\award\Downloads\epxlorer.exe`. Using this offset we can run the dumpfiles plugin for that specific virtual address.

~~~ cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.dumpfiles --virtaddr 0xe6892af1f1f0
~~~

Both options will give us the `epxlorer.exe.img` file that we care about for the next steps.

![PID 9920](/images/BelkaCTF7/Task3-1.png)

This is where I initially got stuck as I calculated the MD5 hash off of this file and ultimately learned something new about the behaviour of Volatility 3. Quoting the writeup from Belkasoft:

> Volatility does not care about making the file size correct and often appends extra blocks of zeros at the end, while the exact amount of zeros may be different. Before calculating the hash, you must find out the exact file size to trim it properly.

This is where I pivoted to using MemProcFS to get the solution. Funnily enough, I did actually find all the information I would have needed to solve this using Volatility 3. During my investigation, I had come across emails in the memory image which were relevant to this task and a later one. Finding those emails will be detailed in that later post (Sidenote: I think they should have flipped the order of these two tasks). One of the emails had the exact size (1,111,552) of the executable in bytes.

![Email](/images/BelkaCTF7/Task3-4.png)

From here, I used 010 Editor to select the relevant bytes from the file and saved this to a new file. You can find instructions at [https://www.sweetscape.com/010editor/manual/SavingFiles.htm](https://www.sweetscape.com/010editor/manual/SavingFiles.htm).  

![010 Editor](/images/BelkaCTF7/Task3-5.png)

There are many, many different ways to get the MD5 hash. Some people uploaded the file to Virus Total, AnyRun, or other similar websites. I used [HashMyFiles](https://www.nirsoft.net/utils/hash_my_files.html) from nirsoft.

![HashMyFiles](/images/BelkaCTF7/Task3-2.png)

You could also use the certutil command on Windows to calculate the MD5 hash.

~~~ cmd
certutil -hashfile myfile.exe MD5
~~~

![certutil](/images/BelkaCTF7/Task3-3.png)

### MemProcFS

This task was a lot easier to accomplish using MemProcFS. I am very interested to know how MemProcFS is able to determine the file size. Something to look into in the future. I browsed to the location of the dumped process and calculated the MD5 hash.

~~~ cmd
\\name\epxlorer.exe-9920\modules\epxlorer.exe\pefile.dll
~~~

There are many, many different ways to get the MD5 hash. Some people uploaded the file to Virus Total, AnyRun, or other similar websites. I used [HashMyFiles](https://www.nirsoft.net/utils/hash_my_files.html) from nirsoft.

![HashMyFiles](/images/BelkaCTF7/Task3-2.png)

You could also use the certutil command on Windows to calculate the MD5 hash.

~~~ cmd
certutil -hashfile pefile.dll MD5
~~~

![certutil](/images/BelkaCTF7/Task3-3.png)

## Thoughts

This was a really good task to include and I think a lot of people are going to learn something. Looking at the [scoreboard](https://belkasoft.com/belkactf7/scoreboard) many people had a lot of trouble solving it. I still need to look into why MemProcFS is able to extract the file correctly. It is important to understand how it extracts files to prevent any mistakes or misunderstanding. Is it reading the header info the executable somehow? Also makes me wonder how many people who use Volatility to extract malware samples are not trimming the files.
