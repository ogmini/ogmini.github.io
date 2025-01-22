---
layout: post
title: Diving Deep - LevelDB
author: 'ogmini'
tags:
 - LevelDB
 - exploration
---

While investigating the ChatGPT Desktop application in yesterday's [post](https://ogmini.github.io/2025/01/20/David-Cowen-Sunday-Funday-ChatGPT.html), I came across an Electron App leveraging LevelDB databases. That of course led me to search for tools and research to help me parse and understand the LevelDB files. 

I found that a lot of research has been focused on Discord, WhatApp, and older versions of Teams. 

- A presentation by Claudia Squire on Electron Apps
    - [https://www.sans.org/presentations/electron-apps-unplugged-unveiling-digital-forensic-treasures/](https://www.sans.org/presentations/electron-apps-unplugged-unveiling-digital-forensic-treasures/)
    - [https://www.youtube.com/watch?v=XDFAgns90-s](https://www.youtube.com/watch?v=XDFAgns90-s)
- Research published by CCL Solutions Group along with python code
    - [https://www.cclsolutionsgroup.com/post/hang-on-thats-not-sqlite-chrome-electron-and-leveldb](https://www.cclsolutionsgroup.com/post/hang-on-thats-not-sqlite-chrome-electron-and-leveldb) 
    - [https://www.cclsolutionsgroup.com/post/indexeddb-on-chromium](https://www.cclsolutionsgroup.com/post/indexeddb-on-chromium)       
    - [https://www.cclsolutionsgroup.com/post/chromium-session-storage-and-local-storage](https://www.cclsolutionsgroup.com/post/chromium-session-storage-and-local-storage) 
- LevelDB Dumper 
    - [https://github.com/mdawsonuk/LevelDBDumper](https://github.com/mdawsonuk/LevelDBDumper)
- Hindsight
    - [https://dfir.blog/hindsight-better-leveldb-and-new-web-ui/](https://dfir.blog/hindsight-better-leveldb-and-new-web-ui/)
- Arsenal Recon has a tool called LevelDB Recon. This is commercial software, I have no way to play with this one.
    - [https://arsenalrecon.com/additional-products](https://arsenalrecon.com/additional-products)
- LDB Reader
    - [https://github.com/pbrudny/ldb-reader](https://github.com/pbrudny/ldb-reader)

There is a lot to dig into here. I haven't found anytime to explore any of the tools or libraries to see how they work. I was saddened to not find any 010 Editor Binary Templates. Maybe something to play around with... The documentation from CCL Solutions Group should make it pretty straightforward.




