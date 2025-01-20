---
layout: post
title: David Cowen Sunday Funday Challenge - ChatGPT Desktop Artifacts
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

IN PROGRESS

## Test Setup

I am performing testing/validation using a Hyper-V VM running Windows 11 Pro 24H2 OS Build 26100.2894.

ChatGPT Desktop Details:
> name: ChatGPT  
> version: 1.2025.16  
> locale: en-US  
> os_name: Windows_NT  
> os_version: 10.0.26100  
> arch: x86_64  

The following tools are being used:

- FTK Imager 4.7.3.81
- Process Monitor
- Strings
- Registry Explorer
- 010 Editor
- DB Browser for SQLite
- ChromeCacheView
- Hindsight


### Steps

1. Install ChatGPT via the Windows Store
2. Sign into ChatGPT
3. Change settings
    - My GPTs
    - Customize ChatGPT
    - Settings
4. Perform various actions 
    - Start Chat
    - Attach Word Document
    - Use voice mode
    - Temporary Chat Flag

At each step of the way, we will monitor for changes to the system using Process Monitor. The `C:\Users\User\AppData\Local\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0` folder will also be copied using FTK Imager for further analysis.

## Preliminary Analysis

Initial search using [strings](https://learn.microsoft.com/en-us/sysinternals/downloads/strings) is unable to find anything related to login. We might need to look into the browser's artifacts to find anything. Maybe they have a saved credential in the browser.

Strings does find files containing words that were part of a chat I had with ChatGPT on "snow" in the following locations:  

- `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb`
- `%localappdata\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Local Storage\leveldb`

Both of these are LevelDB databases. They also appear to log both sides of the conversation. Have to find a tool that can view/parse these into something more structured and readable. Viewing some of the files with 010 Editor makes it very obvious that conversations are contained within. 

Preliminary glimpses at the registry hives using RegistryExplorer do not find anything of note. This isn't too unexpected as I haven't made any changes to settings or defaults. If this behaves similarly to Windows Notepad, registry records will only be created when the default value for a setting is changed. 

## Freeform/Mess Notes

This appears to be a standard electron application installed by the Windows App Store. So far I've identified the following objects:

- a `setting.dat` file just like I described in my research into Notepad [https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings). 
- a Helium container folder holding User and UserClasses registry hives. 
- various SQLite databases
- various leveldb databases

Login is handled by the user's default browser.  


%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0

https://github.com/microsoft/terminal/discussions/13750

Electron App

https://github.com/google/leveldb


