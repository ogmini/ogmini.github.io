---
layout: post
title: BelkaCTF 7 - AAR Command and Control
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task6.png)

We've identified the malware and author. It is now time to do some malware analysis! Where/how is this malware communicating? I'd argue we were given a bit of a clue by knowing that it communicates using a Telegram API.

## MemProcFS

Using MemProcFS, we can browse to the `dns.txt` file to see cached DNS information contained within the memory.

~~~ cmd
M:\sys\net\dns\dns.txt
~~~

Opening that file gives us a hit on a url with Telegram in it - `api-telegram-org.ctf.do`.

![dns.txt](/images/BelkaCTF7/Task5-1.png)

This does get us the correct answer. I argue that it isn't a good solution as we only know this might be the answer because of the Telegram hint. There could be legitimate programs that hit a Telegram API. Nothing at this point has really pointed us to Telegram being used by the malware. You should do some more digging to corroborate this information which we are going to do next.

## PEStudio / dnSpy

Next, I loaded the file into PEStudio to get some information about the file before doing more detailed analysis. It can give me a good idea of what we are working with and inform next steps.

![PEStudio](/images/BelkaCTF7/Task5-2.png)

![PEStudio - Indicators](/images/BelkaCTF7/Task5-3.png)

A few things immediately leap out at me:

- Namespaces
  - Telegram.Bot
  - Costura - This is [Fody.Costura](https://github.com/Fody/Costura) which can be used to create single file executables.
- .NET information in the header
- v4.0.30319 which is .NET Framework 4.0
- mscoree.dll in the libraries
- indicators
  - file > signature

All this points to a .NET executable which we can analyze with a tool like dnSpy. We don't have to dig very far as this is a VERY simple program. Our program's entry point is Form1 and this is where I would be chastising my programmer at work for not changing the default name to somethimg meaningful. This is malware though so best practices don't matter. Right at the top, we can see a readonly string called `custom_api_url` being set to `https://api-telegram-org.ctf.do`.  

![dnSpy](/images/BelkaCTF7/Task5-4.png)

Ripping this malware apart is fun just to see what it does. I'm going to conveniently skip over the next section of code as it is part of the solution for a later task and that writeup will be coming. We can see that the malware attempts to gain persistence by writing to the RUN key calling itself "melok" with the path to the malware.

![Persistence](/images/BelkaCTF7/Task5-5.png)

It also has a self-destruct function that will delete the RUN registry key it created and also delete the executable itself.

![Self Destruct](/images/BelkaCTF7/Task5-6.png)

Using MemProcFS to grab the NTUSER.dat file, we can load this up in Registry Explorer and confirm the existence of this key.

![Run Registry Key](/images/BelkaCTF7/Task5-7.png).

## Thoughts

This was a fun task as a developer as we get to rip someone's code apart. No serious attempts at obfuscation beyond what we'll see in the future task writeup. Volatlity 2 had a dns cache plugin which I do not believe exists in Volatility 3 or at least I couldn't find it. Some people also utilized tools like ANY.RUN to analyze the malware automatically which definitely worked.
