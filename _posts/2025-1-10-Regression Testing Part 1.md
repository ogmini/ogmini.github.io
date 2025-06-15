---
layout: post
title: Forensics Software - Automated Regression/Version Testing Part 1
author: 'ogmini'
tags:
 - research
 - windows-notepad 
---

I'm looking for some advice on how to automate regression testing for forensics software. As I researched Windows Notepad, Microsoft continually updated and made changes to how the binary files worked. I touched upon this issue in a previous post [https://ogmini.github.io/2024/11/27/Microsoft-Store-Older-Versions.html](https://ogmini.github.io/2024/11/27/Microsoft-Store-Older-Versions.html). 

I ended up writing a simple C# console application that utilizes the API located at [https://store.rg-adguard.net/](https://store.rg-adguard.net/). It checks daily for a new version of Windows Notepad or any other application I specify. If one is detected, it downloads and archives the installer for me. At some point, I plan on adding some sort of automated notification so I know that a new version is detected. 

With that "problem" solved, I've been thinking about how to automate testing to some extent so that I can run something and it tells me if the format of the binary file has changed from the expected. My ideal scenario would be:

1. New version of Windows Notepad detected
2. Install new version of Windows Notepad on VM
3. Execute AutoIt or other scripting to simulate usage of Windows Notepad to create known binary files
4. Execute validation program to check for changes
5. Record and send results

All of this would happen automatically and I would just get an email of the results. This scenario could also be extrapolated to test against all prior Windows Notepad versions when I make changes to the [Notepad State Library](https://github.com/ogmini/Notepad-State-Library).

As of today, I've completed Steps 1, 3, and 4. The validation program is barebones but works enough for my purposes. It would be nice to extend it to be a little more modular and configurable. As it stands, the tests performed by the validation program are hardcoded. I'm in the process of possibly having it query a database for tests and their expected results/outputs. I've yet to decide if it is worth the work. 

Step 5 should be fairly easy, my plan is to just leverage a local SQLite database to record test run results. Step 2 is where I'm still researching and deciding on a path. Do I leverage Hyper-V or VirtualBox or other? 

The big question in my head, am I reinventing the wheel? Does something that can do this already exist? Testing your forensic tools was something hammered into my head during my coursework. In those courses no automated solution or tool was suggested beyond the need to test. Automated regression testing and unit tests exist in the software development world and may be something I can leverage here. 

