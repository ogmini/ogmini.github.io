---
layout: post
title: Revisiting Prefetch
author: 'ogmini'
tags:
 - DFIR
 - Research
 - Prefetch
---

My 4/20 [post](https://ogmini.github.io/2025/04/20/Beyond-Sunday-Funday-Shimcache-Amcache-MUICache-Prefetch.html) has set me on a path to revisit Windows Artifacts. I'm going to start with revisiting Prefetch by looking at existing research to gain a base of understanding. 

One complaint I have about looking for pre-existing research is the prevalence of articles that just tell you how to use a tool to get artifacts. There is no explanation of the underlying artifacts and how they work or the explanation is very shallow. You often need to go digging into academic papers or other writeups to get deeper details. I've listed some useful references for Prefetch below and those authors have consistently written in detail about various other artifacts. 

One interesting aspect to me of the Prefetch files is the hash in the filename that can provide the original path to the executable that resulted in the Prefetch file. Something to play around with and test. I'm intrigued to test if Windows 11 uses the same Hash function as Windows 10. None of the current research seems to state one way or the other. It has changed over time since Windows XP.

The prefetch file format has also changed over time and there are still some unknown bytes. Maybe another thing to poke around. It is interesting to see that there are two variants for Windows 11. 

On a sidenote, someone picked up some of my research on Windows Notepad Rewrite [https://0xsp.com/offensive/the-hidden-risk-compromising-notepad-cowriters-bearer-tokens/](https://0xsp.com/offensive/the-hidden-risk-compromising-notepad-cowriters-bearer-tokens/). Very cool writeup and results. 


## References

- [https://github.com/libyal/libscca/blob/main/documentation/Windows%20Prefetch%20File%20(PF)%20format.asciidoc](https://github.com/libyal/libscca/blob/main/documentation/Windows%20Prefetch%20File%20(PF)%20format.asciidoc)
- [https://hiddenillusion.github.io/2016/05/10/go-prefetch-yourself/](https://hiddenillusion.github.io/2016/05/10/go-prefetch-yourself/)
- [https://www.hexacorn.com/blog/2012/06/13/prefetch-hash-calculator-a-hash-lookup-table-xpvistaw7w2k3w2k8/](https://www.hexacorn.com/blog/2012/06/13/prefetch-hash-calculator-a-hash-lookup-table-xpvistaw7w2k3w2k8/)
- [https://github.com/harelsegev/prefetch-hash-cracker](https://github.com/harelsegev/prefetch-hash-cracker)


