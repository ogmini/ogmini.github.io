---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "Dead Portrait Society"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## Dead Portrait Society

Title: Dead Portrait Society     
Description: When was the last written record created?

This challenge was under the iOS section and worth 75 points making it the hardest challenge in the section. It was so difficult, I didn't even know where to begin with this one. I decided not to devote much time to this one as I wanted to solve other more obvious challenges.

### My Process

- Confusion
- Give up and move on

### Actual Solution

The hint of "Portrait" was supposed to guide you into finding the "PersonalizationPortrait" database or "PPSQLDatabase.db". I found a writeup at the following link about this artifact after the solution was posted:

[https://www.mac4n6.com/blog/2020/6/2/guest-post-by-bizzybarney-a-peek-inside-the-ppsqldatabasedb-personalization-portrait-database](https://www.mac4n6.com/blog/2020/6/2/guest-post-by-bizzybarney-a-peek-inside-the-ppsqldatabasedb-personalization-portrait-database)

Checking this database for the last written record would give you the relevant timestamp of 2024–11–20 20:02:02.

### Lessons Learned

1. Need to expand my knowledge of iOS artifacts and its structure. I'm far more comfortable with the file structure and layout of Windows and to some extend Android due to its similarity to Linux.
2. CTFs are a good way to highlight knowledge niches or potentially obscure artifacts to a wide audience.

