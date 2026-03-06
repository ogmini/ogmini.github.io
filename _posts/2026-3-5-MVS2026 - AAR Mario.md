---
layout: post
title: Magnet Virtual Summit 2026 CTF - AAR "That's not a Mario character"
author: 'ogmini'
tags:
 - CTF
 - Challenges
 - Writeups
 - ILEAPP
---

This year I'm going to writeup AARs for the challenges that I solved differently from Hexordia or felt needed to be expanded upon due to my approach or thinking. The first one is "That's not a Mario character".

![iphone-11.png](/images/MVS2026/iphone-11.png)

## That's not a Mario character

Title: That's not a Mario character  
Description: What is the mascot of the frozen dessert shop that the user visits a lot?

### Thoughts/Hints/Background

- Contemplate the hint of "Mario" and "user visits a lot"
- Must be related to location data

In the Cipher Pre-Challenges, one of the answers was DOUBLEBLAK and Ian Whifflin has some great research on Location data. Specifically [https://doubleblak.com/blogPost.php?k=Locations](https://doubleblak.com/blogPost.php?k=Locations) is of relevancy for this challenge. His work was also published on DFIR Reviw [https://doi.org/10.21428/b0ac9c28.9031561b](https://doi.org/10.21428/b0ac9c28.9031561b)

### Solution

Where can we find information about a place that a user visits a lot? Apple Map Trips or Significant Locations! In my case, I checked Apple Map Trips under Magnet Axiom. The solution written by Hexordia used the Significant Locations section. They are both closely related artifacts.

![iphone-solution-11.png](/images/MVS2026/iphone-solution-11.png)

We can see some Latitude/Longitude coordinates and mapping them in Google Maps and searching for ice cream gives us a hit on "Shy Guy Gelato". An obvious nod to the hint of "Mario Character".

![iphone-solution-11-1.png](/images/MVS2026/iphone-solution-11-1.png)

Checking their website results in finding the Panda mascot.

![iphone-solution-11-2.png](/images/MVS2026/iphone-solution-11-2.png)

Answer: Panda

### Takeaways

iLEAPP didn't parse these artifacts so I submitted a PR [https://github.com/abrignoni/iLEAPP/pull/1438](https://github.com/abrignoni/iLEAPP/pull/1438) based on the artifacts parsed by Magnet AXIOM and the research from Ian Whifflin. I would love to do some more research/testing on these Location artifacts. Unfortunately, I have no test devices or a way to obtain images from them.
