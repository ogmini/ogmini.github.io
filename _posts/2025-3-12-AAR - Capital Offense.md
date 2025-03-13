---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Capital Offense"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## Capital Offense

Title: Capital Offense   
Description: Decode the secret message

This challenge was under the Cipher section and worth 50 points.  

### My Process

- Opened file in 010 Editor at a loss where to go
    
### Actual Solution

For this solution, I learned a lot from BlueMonkey4n6's Youtube video [https://www.youtube.com/watch?v=EZKeMP2kM9c](https://www.youtube.com/watch?v=EZKeMP2kM9c). They really broke down the steps they took to solve this challenge. By examining the file in a hex editor it is possible to notice some recognizable strings that appear reversed from the typical. Most importantly, the headers for a PNG file appear at the end of the file. Typically they would show up as `89 50 4E 47 0D 0A 1A 0A` at the start; but in this case we see `0A 1A 0A 0D 47 4E 50 89` at the end. There are also some other telltale signs related to various meta data for PNGs that appear as reversed strings. The IEND was also visible at the start of the file but reversed and is used to signify the end of a PNG file. At this point, it is just a matter of reversing all the bytes back to how they should be. BlueMonkey4n6 ends up using xxd to pipe the output to tail to do the reversing and piping that output back to xxd to save as a PNG. Andro6 ended up just using CyberChef and uploading the file. Two different methods and I've found a third method for my preferred hex editor. Ramprasad Ramshankar wrote a useful 010 Editor script to reverse the byte order which you can find here [https://github.com/diyinfosec/010-Editor/blob/master/ReverseFileEndianness.1sc](https://github.com/diyinfosec/010-Editor/blob/master/ReverseFileEndianness.1sc).

### Lessons Learned

1. CyberChef can really make things easy
2. Look for patterns and strings that are recognizable. Experience will help in recognizing file headers. 
