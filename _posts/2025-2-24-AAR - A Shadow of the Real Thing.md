---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "A Shadow of the Real Thing"
author: 'ogmini'
tags:
 - CTF
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge.

## A Shadow of the Real Thing

Title: A Shadow of the Real Thing  
Description: What is the hashed password for the user "chick"?

This challenge was under the Windows 11 section and worth 25 points making it around a medium difficulty.

### My Process

- NTLM Hash!
  - Used multiple tools including [impacket](https://github.com/fortra/impacket), Magnet AXIOM, and [mimikatz](https://github.com/ParrotSec/mimikatz) to retrieve from Registry Hives. Am I using the tools wrong? They all agree on the hash.
- "Shadow" must be a hint...
  - Are we supposed to be looking at Volume Shadow Copy for a previous password's hash? It would be the shadow of the real password assuming it was changed... Go down a rabbit hole of trying to get the Volume Shadow Copy from the image and failing. I never verified if the user's password was ever changed.

### Actual Solution

The Kali Linux WSL and vhdx file! This one really stung as I had specifically noticed and called out the existence of this in my [pre-analysis](https://ogmini.github.io/2025/02/12/Magnet-CTF-Pre-Analysis.html) and hadn't yet used that knowledge in solving any of the challenges. This will actually be a recurring theme for later "fails" and AARs. Noticing and making note of "interesting" artifacts but not actually connecting the dots or using them.

Linux often utilizes a shadow file for authentication. This file contains usernames, an encrypted/hashed password among other pieces of information. It was as simple as grabbing the shadow file and opening it up as they only asked for the hash.

[https://www.man7.org/linux/man-pages/man5/shadow.5.html](https://www.man7.org/linux/man-pages/man5/shadow.5.html)

### Lessons Learned

1. Look at your pre-analysis notes and make note of what you haven't used. Remember the [Duck Test](https://en.wikipedia.org/wiki/Duck_test).
2. Don't get fixated! Again, I got fixated on "Shadow" meaning Volume Shadow Copy. I didn't even check if the user had changed their password.
3. Have a healthy trust of my knowledge/skills. I didn't trust that I was using the tools right and must be pulling the wrong NTLM hash even after using multiple tools that agreed with each other.
