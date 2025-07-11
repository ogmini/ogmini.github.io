---
layout: post
title: Windows Notepad - Recent Files (New Option)
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

I need to get back to documenting the changes in Windows Notepad. I left off on version 11.2410.20.0 with my last [post](https://ogmini.github.io/2025/04/27/Windows-Notepad-Version-Changes-(11.2049.9.0).html) back on 4/27/2025. I'm going to skip ahead as Microsoft added a new "Recent Files" option. I have not yet identified which specific version introduced the change. I'll go back to that project at some point.

![Recent Files Option](/images/windowsnotepad/recent_files.png)

![Recent Files](/images/windowsnotepad/recent_list.png)

Windows Notepad keeps the recent list in the `settings.dat` which I've previously documented at [https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings). This file is located at `%localappdata%\Packages\Microsoft.WindowsNotepad_8wekyb3d8bbwe\Settings` and is an application hive. Two new keys were added:

| KeyName | Type | Notes |
| --- | --- | --- |
| RecentFiles | 0x5f5e10c | CSV array. List is in descending order with the most recently closed file at the top. |
| RecentFilesEnabled | 0x5f5e10b | 0 = Off / 1 = On |
| RecentFilesFirstLoad | 0x5f5e10b | 0 = Off / 1 = On |

The items in the list are the full path to the file.

![Recent Files Registry](/images/windowsnotepad/recent_registry.png)

## Behaviour

- Only keeps the last 10 recently closed files
- Updates on closing a file and not when Windows Notepad itself is closed

## Importance

This artifact is seperate from the RecentFiles/RecentDocs registry entries and other artifacts that can show file/folder interactions. It can be used to validate other evidence and/or show evidence of a user attempting to hide their tracks. A user may clear the Recent Files while not being aware of the Recent Files in Windows Notepad and vice versa.
