---
layout: post
title: Reverse Engineering Rewrite API
author: 'ogmini'
tags:
 - Rewrite-API
 - Reverse-Engineering
---

Last week I started poking around the network traffic generated by Windows Notepad when it makes calls to Rewrite. I made a [post](https://ogmini.github.io/2025/03/18/Windows-Notepad-Rewrite-Part-4.html) about what I found using Wireshark and mitmproxy. Part of the purpose of this blog is to push my knowledge by forcing me to actually DO and not just read and learn theory. That last post on mitmproxy and Wireshark was a perfect example of the DO. I had known that it was possible to setup a proxy to intecept and decrypt traffic. I even knew the general steps and tools required to analyze that traffic. What I never had was the need to put this knowledge to practice. I can now say that I've done it.

The findings from that post also left me with the question if I could make my own calls to the Rewrite API without using Windows Notepad. This is a little outside of my current knowledge and perfect chance to learn new skills. Can I reverse engineer the Rewrite API and utilize it in other ways?

I've worked with Postman in my professional life to troubleshoot/test APIs. It is now time to use it in a slightly different way to reverse engineer an API. I could also possibly use Burpsuite. First step is just to replay a request and get back the expected response. After that, we can start playing around by modifying the request.

Any advice is greatly welcomed. I am venturing outside of my comfort area.
