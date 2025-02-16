---
layout: post
title: Investigating Visual Studio Code
author: 'ogmini'
tags:
 - DFIR
---

I've begun to look at Visual Studio Code in my quest to document useful digital artifacts in various text editors. In a similar fashion to Windows Notepad and Notepad++, Visual Studio Code will keep unsaved content between sessions. After documenting what I can find I will use that knowledge to see how one might attack Visual Studio Code. Another project is slowly building in my head to write a unified application to attack text editors and their data. 

What I've found so far is that Visual Studio Code stores the unsaved content in `%appdata%\Code\Backups\`. There is a folder named with a Unix Filetime that contains folders which contains files that hold information about tabs with unsaved content. These files are straight text with the first line containing a file location and a JSON object. Later lines contain the text from the tab. If a tab isn't saved, the first line isn't present. 

More research to come! I've been really sucked into the Magnet CTF and trying to complete more of the challenges.


