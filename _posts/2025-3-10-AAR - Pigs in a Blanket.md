---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Pigs in a Blanket"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## Pigs in a Blanket

Title: Pigs in a Blanket      
Description: Decode the secret message

This challenge was under the Cipher section and worth 25 points.  

### My Process

- I opened the text file and really didn't know where to begin.
- I half heartedly threw the string at various AI chatbots and asked them to identify the encoding.
    
### Actual Solution

I'm learning from Andro6's writeups located at [https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b](https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b). A lot of the cipher challenges appear to rely on recognizing what encoding is being used either via the hint or just familiarity. In this case, the hint was "Pigs" which would point to the [Bacon cipher](https://www.dcode.fr/bacon-cipher). But, the text doesn't look like any Bacon cipher to me... There aren't any obvious sequences of two different character grouped into fives. As the dcode website states though, over-encryption is often used to help protect the message. 

So, what over-encryption is being used? [ASCII85](https://www.dcode.fr/ascii-85-encoding). From reading up on ASCII85, it is very suited for encoding strings that multiples of 5 in length. Which is perfect for the Bacon cipher. It also only uses a subset of the ASCII characters, specifically 33-126. Knowing these things, it might have been easier to identify this as an ASCII85 encoded string. 

### Lessons Learned

1. Learned about ASCII85
2. ASCII85 is used in Adobe PDF/PS files
3. Need to get more comfortable with dcode
