---
layout: post
title: BSides NYC 0x06 - CTFd Challenges
author: 'ogmini'
tags:
 - CTF
 - BSidesNYC
---

Another year, another great BSides NYC! I volunteered with [CTFd](https://ctfd.io/) again and contributed some Linux Forensics challenges. Last year centered around specific digital artifacts and understanding how to interpret them. This year, I wanted to create a forensic image of a system that was compromised and have the partipants piece together what happened. One limitation was keeping the image size under 100MB. I had to be ruthless in cutting files and removing "unimportant" files that were not directly related to the challenges. So maybe consider this a triaged acquisition.

I'm unsure if the challenges can still be completed at [https://bsidesnyc.ctfd.io](https://bsidesnyc.ctfd.io). If not, you can find the image file here:

[filesystem.tar.gz](/files/bsidesnyc0x06/filesystem.tar.gz)

The MD5 hash is 0af0908454c2516a81fa8d7c455cfe2d.

## Challenges

Ideally, you solve these in order as later challenges do give a hint at the answer to previous ones. The CTFd instance had pre-requirements configured to unlock further challenges.

### No SSH key file?

The BSidesNYC CTFd staff setup a server to host various files for the event. In the process of setting it up, they noticed some unusual activity and have called you in to investigate. They have provided you with a dump of the filesystem from the server.
  
What was the IP of the first SSH login?
  
FORMAT: ###.###.###.###

### What are you chasing?

What software was installed?
  
FORMAT: Single word

### How apt

When did the software finish installing?

FORMAT: yyyy-mm-dd  hh:mm:ss

### Who does Number 2 work for?!

What was the IP of the second SSH login?
  
FORMAT: ###.###.###.###

### The Dead Drop

What feature was enabled in the installed software by the second SSH login?
  
FORMAT: Single word

### Drop it like its hot

How many files were dropped on the server by the second SSH login?
  
FORMAT: #

### The Boss?

What is the LoginName of the Tailnet owner?
  
FORMAT: Email Address

### Follow the path

What is the folder location of the deaddrop?
  
FORMAT: /PATH/TO/FOLDER

### Secret Message

Who signed the secret message?
  
FORMAT: {something}

### Nice Desk!

What was the secret message in the photograph?
  
FORMAT: {something}

## Answers

Feel free to reach out to me on LinkedIn at [https://www.linkedin.com/in/christophereng/](https://www.linkedin.com/in/christophereng/)

## Thoughts

I thoroughly enjoyed BSidesNYC 0x06. The team did another great job and I understand that attendance was even higher this year. Already starting to think about challenges for next year.

I learned some more things about linux forensics by making these challenges. I specifically chose to make linux forensic challenges because most of my experience has been Windows or Android based. Performing and documenting actions and subsequently looking for digital artifacts related to those actions is a great way to learn and discover things. You can't make good challenges if you can't solve them yourself!

![AI Vending Machine](/images/bsidesnyc0x06/gimmeflag.PNG)

This photo was from me solving the AI Vending Machine challenge with the first prompt. I didn't get a picture of the actual vending machine. The challenge was to get it to vend you the flag. You only had a balance of $10 and the flag cost $100. Evidently, you can just walk in like your own the place and get what you want.
