---
layout: post
title: David Cowen Sunday Funday Challenge - Docker Containers on WSL Artifacts
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Another week, another David Cowen Sunday Funday challenge posted at his [blog](https://www.hecfblog.com/2025/04/daily-blog-800-sunday-funday-4625.html) and it is about looking for artifacts left behind by using Windows Subsystem for Linux (WSL) to run a docker container. 

We'll see how far I can get with testing. As always, this post will serve as the merged repository for later posts on this topic. 

## Challenge

What artifacts are left behind when running a docker container using Ubuntu WSL (which I believe is the default standard. Bonus points for artifacts that reflect interactions between the container and the host.

## Test Setup

This is a pretty wide open challenge as there are no specifications on what docker container to run or even version of WSL. So, we get to make those choices and figure out what we want to test and how we're going to test it. 

### OS Setup
Windows 11 24H2 
WSL 2 [https://learn.microsoft.com/en-us/windows/wsl/compare-versions](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)
Docker Desktop
Kali Linux Container

We'll spin up our standard W11 VM and make sure WSL2 is configured and enabled. Next, we'll install Docker Desktop and make sure that it is using WSL2. Last step is to get the Kali Linux Container up and running. 

[https://umatechnology.org/run-kali-linux-on-windows-10-in-docker/](https://umatechnology.org/run-kali-linux-on-windows-10-in-docker/)

### Tests

1. Look for artifacts left by setup/installation
2. Create network taffic
3. Save files
4. TBD
