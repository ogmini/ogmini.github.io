---
layout: post
title: Magnet Virtual Summit 2025 CTF - AAR "YOU Watch a Lot of space Videos"
author: 'ogmini'
tags:
 - CTF 
 - Challenges
 - Writeups
---

![NASA Database](/images/memes/nasabrowser.png)   
NASA has a SQLite DB on your Android Phone?

Continuing with my writeups on my "fails" or the ones I just couldn't figure out in the timeframe alloted. I want to talk about how I went about trying to solve the challenge and where I went wrong. This should help me in the future by highlighting weaknesses and areas for improvement. Each post will focus on just one "fail" challenge. You can find all my writeups [here](https://ogmini.github.io/ctf).

## YOU Watch a Lot of space Videos 

Title: YOU Watch a Lot of space Videos      
Description: What's the numerical rating for the app with the most installs?

This challenge was under the Android section and worth 25 points making it the second to most difficult challenge. 

### My Process

- They must be talking about Youtube as the hint has "YOU" in it. Grab the numerical rating fron the Play Store. 
    - This didn't work and seemed to easy for the points
- Is there a local artifact that stores what someone rated an app?
    - Do some manual searches for databases in locations related to the play store
- Hit Dead end and move on
    
### Actual Solution

Thanks for Andro6 for posting this solution on his blog [https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b](https://medium.com/@andro6.ucsy/magnet-ctf-2025-writeups-fb73793eda8b). I haven't been able to find any documentation explaining the behavior of the specific artifact. 

The other part of the hint mentioned space and was a reference to NASA. Searching for a file with NASA in the name would have led to the discovery of the "nasa_ps_db" which could be opened in DB Browser for SQLite. The "play_app" table provides the numerical rating and the number of installs. 

### Lessons Learned

1. Think creatively with the hints. I hopefully can get better with this by doing more challenges.
2. I keep coming back to wondering if there is an automated way to grab all obvious SQLite databases and add them all to a centralized repository. I could mass query the centralized repository for various attributes like column names, table names, or even values within fields. In this challenge, my theorized workflow would be to query the centralized repository for any hits on "youtube". I'm sure this would result in lots of hits but the query could be refined to look for records that had column names of "rating" or "install". Maybe a future area of development and research. Or this already exists.
