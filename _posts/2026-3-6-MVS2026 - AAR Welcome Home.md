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

![iphone-12.png](/images/MVS2026/iphone-12.png)

This year I'm going to writeup AARs for the challenges that I solved differently from Hexordia or felt needed to be expanded upon due to my approach or thinking.

## Welcome Home

Title: Welcome Home  
Description: What port was the host directed through for a sign-in?

### Thoughts/Hints/Background

- Wifi Network Portal?
- Possibly home network?

### Solution

Let us check browser history for any interesting URLs with a port specified. We can look both in iLEAPP and Magnet Axiom where we find a hit for `http://127.0.0.1:49354/?state=DlZ5NRaWdsqHUiS21f2V&code=4/0ATX87lPypweMwdXMiCRQiFSHNUkw86nymXg8LV6gv 0Tb5fB9gs5iHzJ0fKHicbKzcaMahg&scope=https://www.googleapis.com/auth/flexible-api`. There is an obvious port of 49354 after the IP of 127.0.0.1.

![iphone-solution-12.png](/images/MVS2026/iphone-solution-12.png)

![iphone-solution-12-1.png](/images/MVS2026/iphone-solution-12-1.png)

Answer: 49354

### Takeaways

Interestingly, the iLEAPP plugin from Kevin Pagano reports the Redirect Source and Redirect Direction from the database while Magnet Axiom doesn't show this or at least not by default. I'd like to get confirmation; but it looks like the individual was hosting something on 127.0.0.1 on their local network that utilized Google OAuth 2.0 authorization. I'm intrigued to know what they were hosting...
