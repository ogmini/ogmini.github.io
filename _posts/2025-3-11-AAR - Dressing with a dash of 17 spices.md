---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Dressing, with a dash, of 17 spices"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## Dressing, with a dash, of 17 spices

Title: Dressing, with a dash, of 17 spices    
Description: Decode the secret message

This challenge was under the Cipher section and worth 50 points.  

### My Process

- Play the wav file and it sounds like morse code. I guess that is what the "dash" refers to as Morse code consists of dots and dashes.
- Googling for a morse code decoder results in [https://morsecode.world/international/decoder/audio-decoder-adaptive.html](https://morsecode.world/international/decoder/audio-decoder-adaptive.html)
- I just get numbers back and I'm at a loss at where to go next. The 17 spices must be a hint but I don't understand how the Colonel and his fried chicken has any significance. 
    
### Actual Solution

I'm learning from Andro6's writeups located at [https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b](https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b). A lot of the cipher challenges appear to rely on recognizing what encoding is being used either via the hint or just familiarity. In this case, I was on the right track and had figured out the morse code part of the challenge. The second hint of 17 was a reference to how may characters to shift for the [Caesar Cipher](https://www.dcode.fr/caesar-cipher). The Caeser Cipher is used to shift letters in the alphabet and requires translating the decoded morse code numbers to their representative letters. The letters are than shifted by 17 to get the message. Alternatively, dcode can bruteforce the Caesar Cipher and identifies 17 as the shift.   

Steps:
1. Decode Morse Code message which consists of numbers
2. Convert numbers to letters (A=1, B=2, etc)
3. Decode Caesar Cipher with a shift of 17

### Lessons Learned

1. Learned about online Morse Code decoders
2. Harder cipher challenges will often utilize multiple ciphers/encoding
3. If something decodes to numbers, pay attention to their range as they may represent letters 1-26.
4. dcode can bruteforce Caesar Ciphers
