---
layout: post
title: Windows Notepad vs Notepad++ - Artifact Comparison
author: 'ogmini'
tags:
 - DFIR
 - Windows-Notepad
 - Notepad++
---

It is always interesting to look at two different approaches to the same problem. Both Windows Notepad and Notepad++ are at their core text editors that support tabs and have the ability to keep unsaved changes between sessions. How they approach these features differ and we'll be talking about how these manifest themselves in the digital artifacts.

Just as a quick recap on prior research:

- [Notepad++](https://ogmini.github.io/2025/02/09/Notepad++-Documenting-Digital-Artifacts-Part-2.html)
- [Windows Notepad](https://ogmini.github.io/2024/12/01/Is-that-Windows-Notepad-window-really-empty.html)

One very obvious difference is the simplicity and elegance of how Notepad++ handles tabs and sessions. Everything is handled by staight text files that require no extra knowledge to read or understand. Windows Notepad utilizes various bin files and application hives that require some knowledge and software to easily read or understand. They are also not easy to manually modify.

## Tracking Tabs

| Notepad++ | Windows Notepad |
| --- | --- |
| Tabs are stored in the `session.xml` file. Each element stores information about the relevant tab. All of this is easily readable and editable as this is a straight xml file. | Tabs are stored in the Windowstate bin files. A list of GUIDs relating back to each individual tab is stored within the bin file. No other information about the individual tabs are stored here. |

## Unsaved Content

| Notepad++ | Windows Notepad | 
| --- | --- |
| Unsaved content for Tabs are stored as straight text files with filenames that point back to the specific tab. All of this is easily readable and editable as these are just text. These files are not deleted unless the tab had no content to begin with or the content is saved. Leaves some interesting possibilities for data retrieval. | Tabs are stored in Tabstate bin files with filenames that match GUIDs in the Windowstate bin files. A lot of information is stored here about each Tab. While Windows Notepad is active, changes are actually tracked almost like a keylogger in the unsaved buffer changes. 

## Comments

Windows Notepad leverages multiple CRC checks within the bin files to check for corruption. There is some information that is tracked and stored at least temporarily that I'm unsure of its use. Example would be the unsaved buffer changes that exist while Windows Notepad is open. I find it interesting that Notepad++ seems to keep unsaved content files around indefinitely for potential recovery. I gather it is a feature in case of crashes and it can be configured not to do it. I was pleasantly(?!) surprised to see FILETIME being used in both Windows Notepad and Notepad++. 

It leaves me with the question, why the layer of obfuscation for Windows Notepad? Is it for security? That might explain the gradual removal of some prior digital artifacts that existed in earlier versions of Windows Notepad. I really need to get back to doing that regression research and document the changes. 

