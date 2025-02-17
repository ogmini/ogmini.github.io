---
layout: post
title: Investigating Visual Studio Code - Part 2
author: 'ogmini'
tags:
 - DFIR
---

Still banging my head with the 2025 Magnet CTF and was able to solve a few more today. Let us get back to investigating Visual Studio Code!

Recapping my previous [post](https://ogmini.github.io/2025/02/15/Investigating-Visual-Studio-Code.html), I've already identified one folder of interest located at `%appdata%\Code\Backups` that appears to hold files with the unsaved content from any tabs. Within that folder there is an extra layer of folders called "file" and "untitled". The "file" folder contains the unsaved content of a tab that is resident on disk. The "untitled" folder contains the unsaved content of an unsaved tab. 

The next folder that I've identified is located at `%appdata%\Code\User`. There are SQLite databases located within the "globalStorage" and "workspaceStorage" folders. I haven't had a chance to open them up yet. I'm going to blame working on the Magnet CTF. The "History" folder is very interesting and appears to contain folders for each tab that was ever opened. These folders contain an "entries.json" file that records snapshots of the contents of the corresponding tab. These folders could be a goldmine, from very prelimnary testing it keeps copies of the files from everytime it has been saved. The "entries.json" file is created when Visual Studio Code is closed. The naming of each folder at the moment doesn't make sense; but I'm sure it maps back to something and it is just a matter of putting in the time to figure it out. 

## Recap

| Folder/File | Notes |
| --- | --- |
| %appdata%\Code\Backups | Contains the content for any unsaved tabs. There are seperate folders for unsaved content from files and non-files |
| %appdata%\Code\User | Lots of interesting things contained within. "History" folder appears to keep a copy of the file from every save. | 

