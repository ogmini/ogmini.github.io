---
layout: post
title: Remote Desktop Manager - Artifacts Part 6
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

Another quick post as its Memorial Day weekend! I took a few minutes today to continue looking at what the master key actually encrypts. Yesterday we looked specifically at the tables related to Connections. Today, I'm looking at the tables related to documentation which I documented in [Part 4](https://ogmini.github.io/2025/05/23/Remote-Desktop-Manager-Artifacts-Part-4.html).

Nothing is encrypted here at all by the master key. All the documentation and its history are stored in plain text.

I'm rather surprised at this!
