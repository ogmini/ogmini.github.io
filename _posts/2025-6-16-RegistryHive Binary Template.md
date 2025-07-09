---
layout: post
title: 010 Editor - RegistryHive Binary Template
author: 'ogmini'
tags:
 - Registryhive
 - 010-Editor 
---

Spent a few minutes today making an update to the RegistryHive.bt for 010 Editor. I had previously updated it to handle the application hive types described in this [post](https://lunarfrog.com/blog/inspect-app-settings). I had noticed that when using it to examine the `settings.dat` files for Windows Notepad it would sometimes throw an error on one test system but not another. It was attempting to read bytes at an address that did not exist. I'm unsure why the inconsistency and I want to dig into it a little more. It is possible the Binary Template isn't completely handling the file correctly.  
