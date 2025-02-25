---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Out of the Ordinary"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

![Nesting Dolls](/images/memes/nesting.jpg)   
There is a smaller Operating System inside!

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## Out of the Ordinary

Title: Out of the Ordinary   
Description: What suspicious command line tool was installed on Marys system??

This challenge was under the Windows 11 section and worth 50 points making it the second most difficult challenge. 

### My Process

- Check installed programs
- PowerShell command history
    - Find evident of sdelete.exe being executed and assume this is the answer. 
- No obvious hint
- Start manually searching for suspicious executables. 
    - This is not a sustainable solution. Brute force method.

### Actual Solution

KALI LINUX AGAIN! Are we sensing a trend yet? I really, really lost out big by just seemingly forgetting about the Kali Linux WSL and vhdx file. 

As stated in yesterday's [post](https://ogmini.github.io/2025/02/24/AAR-A-Shadow-of-the-Real-Thing.html), there is a recurring theme here of noticing "interesting" artifacts but not connecting them or using them in the challenges. All three of the Windows 11 Challenges that I failed to solve required knowledge or looking at the Kali Linux artifact.

Linux, more specifically Bash keeps a history of commands that have been executed in the terminal. Solving this challenge required reading that hidden bash_history text file and seeing the installation of steghide using apt install. Funnily enough, the process was the same just for a different Operating System that existed within the first one. Spoiler, there is also history of steghide being used and this solves the last challenge. That writeup will be for tomorrow. 

[https://bashcommands.com/bash-history-file](https://bashcommands.com/bash-history-file)

### Lessons Learned

1. Look at your pre-analysis notes and make note of what you haven't used. Remember the [Duck Test](https://en.wikipedia.org/wiki/Duck_test). 
2. Don't get fixated! I got fixated on Windows 11 and continuted to ignore the Kali Linux artifact.
