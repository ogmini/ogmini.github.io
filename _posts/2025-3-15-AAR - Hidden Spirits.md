---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Hidden Spirits"
author: 'ogmini'
tags:
 - CTF
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [page](https://ogmini.github.io/ctf).

## Hidden Spirits

Title: Hidden Spirits  
Description: Decode the secret message

This challenge was under the Cipher section and worth 75 points making it the hardest challenge.  

### My Process

- Tried using [https://www.aperisolve.com/](https://www.aperisolve.com/)

### Actual Solution

For this solution, I appreciated the writeup from Kevin Pagano at Stark4n6 [https://www.stark4n6.com/2025/03/magnet-virtual-summit-2025-ctf-ciphers.html](https://www.stark4n6.com/2025/03/magnet-virtual-summit-2025-ctf-ciphers.html). There is a specific tool located at [https://www.lddgo.net/en/image/image-hide-text](https://www.lddgo.net/en/image/image-hide-text) that can be used to hide text in an image. I don't know exactly how this works as there doesn't appear to be any published information. Doing some quick testing shows that the tool deletes a bunch of the chunks in the PNG file including tEXt,eXIf, and the original IDAT. Instead, it replaces all that with a bunch of IDAT chunks. This could be an interesting thing to dive into to try to understand how it works.

After using that tool, you end up with a weird sequence that appears to be elvish. The hint of "Spirit" is supposed to point at the Spirit DVD Code which can be decoded with this tool [https://rumkin.com/tools/cipher/spirit-dvd/](https://rumkin.com/tools/cipher/spirit-dvd/).

This was a really tough one. I'd be surprised if anyone solved this WITHOUT looking at Kevin Pagano's website. The hint really only applies to the second part of the challenge and getting there requires a very specific tool with a not very unique name.  

### Lessons Learned

1. Get familiar with more image steganography tools.
2. Importance of "researching your target". Knowing that Kevin Pagano was the author and has a website of useful tools is vital. This would apply to the individual or organization you are investigating. You need to do your research on them to understand them.
3. Learn more about the PNG format and its chunks.
