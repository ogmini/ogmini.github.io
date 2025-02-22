---
layout: post
title: KAPE Target - Windows Notepad WOOPS
author: 'ogmini'
tags:
 - DFIR
 - Windows Notepad
 - KAPE
---

Looks like Andrew Rathburn had already created a Target for Windows Notepad! I somehow didn't notice it because I only checked the Apps folder and not the Windows folder. I've updated it to pull the Window State *.bin files and the settings.dat Application Registry file. I also added a link to my documentation on the file format and behavior.

Just submitted the Pull Request. 

I'll double check there is no Module that already exists. I'm pretty sure I didn't see one. This would be useful for a Live Response as Windows Notepad Parser would be able to acquire the [Unsaved Buffer Chunks](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#unsaved-buffer-chunk) that might be present in the Tab State *.bin files. These would be lost on shutdown of Windows Notepad or Windows.
