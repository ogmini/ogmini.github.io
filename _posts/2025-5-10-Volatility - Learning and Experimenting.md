---
layout: post
title: Volatility - Plugin?
author: 'ogmini'
tags:
 - DFIR
 - Volatility
 - Memory-Forensics
---

I've been wanting to dabble more with Volatility beyond the standard CTF or assignments that I had in my courses. I recently came across a [post](https://infosecwriteups.com/extracting-an-unsaved-memory-content-by-walking-through-windows-heaps-but-how-6992589d872e) talking about using Volatility to recover text from Notepad (The old version) for a CTF Challenge. I think it would be a fun exercise to write a Volatility plugin to specifically scan and parse out the Unsaved Buffer Chunks from active Windows Notepad sessions.    

I find this is the best way to learn and experiment. It builds upon a base of knowledge that I already have and adds something new to the mix. This is the exact reason I submitted Kape Targets and Modules. I wanted to learn about Kape beyond just using it as a tool. Now, I want to write a Volatility plugin and go beyond just using the tool. Knowing how to use a tool is great; but improving that tool is way more powerful. 

An interesting thought about using memory analysis on Windows Notepad is the application in an EDR solution. Could scanning the memory allow alerting on potentially confidential or protected data being typed in Windows Notepad? Imagine an individual typing a Social Security # into Windows Notepad and an alert being sent or even the process being killed.  
