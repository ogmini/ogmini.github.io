---
layout: post
title: David Cowen Sunday Funday Challenge - Docker Containers on WSL Artifacts - Part 2
author: 'ogmini'
tags:
 - sunday-funday
 - challenge
---

Getting some work done on this challenge; but I am not very confident that I'll find much before the deadline. As a quick note for anyone else trying to run Docker or really anything that results in nested virtualization. You need to set ExposeVirtualizationExtensions to true for Hyper-V [https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/user-guide/nested-virtualization). 

As always, I'm using Process Monitor, Process Explorer, and TCPView to keep an eye on activity while performing testing. Some locations of interest that have already surfaced:

| Folder Location | Comments |
| --- | --- |
| `%localappdata%\Docker\log` | Contains logs |
| `%localappdata%\Docker\wsl` | Contains vhdx files related wot WSL and Containers |
| `%userprofile%\.docker\contexts\meta` | |

The `com.docker.backend.exe.log` file is of great interest as we can see all the events going on with containers and so forth. For example the line:

> [2025-04-10T20:27:47.539553700Z][com.docker.backend.exe.ports] exposer.Add(de1d1ec90a81dff7dc48705fbf17c81ec6c4717da6df1056f65360e8049aacd9, /great_matsumoto, [])

Shows us the running of a container which got the name of great_matsumoto. 

![Matsumoto Frog](/images/memes/matsumoto.jpg)   

That's as far as I got—there's still more to look into, but it's a start.