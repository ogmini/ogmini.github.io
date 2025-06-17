---
layout: post
title: Windows Notepad - Rewrite / AI Part 5
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
---

Continuing from [Part 4](https://ogmini.github.io/2025/03/18/Windows-Notepad-Rewrite-Part-4.html) on researching Windows Notepad - Rewrite. Taking a little detour and looking at the Correlation Vector. I don't think this will be useful for anything; but I'll doument what I'm seeing. Maybe someone will recognize this or knows more than me and can reach out with more details!

## Correlation Vector

The specifications for the correlation vector can be found on Microsoft's GitHub at [https://github.com/microsoft/CorrelationVector/blob/master/cV%20-%202.1.md](https://github.com/microsoft/CorrelationVector/blob/master/cV%20-%202.1.md).

In short, it is used to track events across distributed systems. So maybe there is a way to use it to correlate events found within logs? The correlation vector consists of a Base and a Vector where the Base is a 128-bit Base64 encoded value with the typical "==" at the end removed. The Vector consists of "." delimited elements. An example of a correlation vector that we see in the network traffic is:

NI01LloYmku1iDGUgN3LXQ.1

"NI01LloYmku1iDGUgN3LXQ" is the Base and the "1" is the first Vector. I'm unsure if the Base decodes and maps back to anything useful or human readable. The vector does increment by 1 in each API call and response during a session. A session exists for all the calls to Rewrite while Windows Notepad is open. Closing and reopening Windows Notepad results in a new correlation vector with a new Base and the Vector will reset.

## Conclusion

Is this useful? I doubt it. Is it interesting? Yes, it is always interesting to see how things work. 

