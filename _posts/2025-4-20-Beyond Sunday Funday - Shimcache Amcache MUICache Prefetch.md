---
layout: post
title: Beyond Sunday Funday - Revisiting Shimcache, Amcache, MUICache, and Prefetch
author: 'ogmini'
tags:
 - DFIR
 - Research
---

![Another One](another_one_cowen.png)

"Another One" - David Cowen, [https://www.hecfblog.com/2025/04/daily-blog-813-solution-saturday-41925.html](https://www.hecfblog.com/2025/04/daily-blog-813-solution-saturday-41925.html) that involved looking at artifacts left behind browser password tools. This reminded me that most of my understanding of Shimcache, Amcache, MUICache, and Prefetch is from other people's research, writeups, and classes. I have never personally done any verification or thorough testing. Maybe things have changed in Windows 11? The best way to truly know an artifact is to test it yourself.

Unless another one of David Cowen's Sunday Funday Challenges takes precedence. I'm going to start researching and testing these artifacts in order to gain a more intimate amount of knowledge on them. The plan is to understand each artifact as they stand on their own. What causes their creation, modification, and deletion/removal. Can these be manipulated? How? Can this manipulation be detected? Next, I want to look at all four of these artifacts together in total. If one exists, should another also exist? Which ones can/should exist alone? 


