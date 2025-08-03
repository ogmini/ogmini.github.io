---
layout: post
title: Velociraptor - Artifact Windows Notepad - Part 2
author: 'ogmini'
tags:
 - DFIR
 - Windows-Notepad
 - Velociraptor
---

That was a very quick turnaround from my previous [post](https://ogmini.github.io/2025/07/30/Velociraptor-Artifact-Windows-Notepad.html) and opening an [issue](https://github.com/Velocidex/velociraptor/issues/4372) on the Velociraptor repository. There is now an official artifact from this [commit](https://github.com/Velocidex/velociraptor/pull/4375) by Mike Cohen.

I still want to get better acquainted with Go and will be dissecting how they added varint support to the binary parser.

He did find my half finished test cases which initially caused some confusion as they appear to be messed up or at least out of date. This did give me the push to revisit my testing data and testing process. I never finished it. I've generated new testing data based on the current version of Windows Notepad (11.2504.62.0) and added it to the repository.

My new testing plan for when a new version is released:

1. AutoIt Script to generate TabState/WindowState files with known inputs/outputs.
2. Capture TabState/WindowState files.
3. Run xUnit Tests

This should hopefully let me detect any changes to the files that break the parser. I continue to be particularly interested in the [Configuration Block](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#configuration-block), mainly the More Options. If I see that value go above 3, it will be an exciting time. Now, it is just a matter of DOING IT.
