---
layout: post
title: Windows Notepad - Modifying TabState or WindowState Files
author: 'ogmini'
tags:
 - windows-notepad
---

Had an [issue](https://github.com/ogmini/Notepad-State-Library/issues/2) submitted to the Notepad State Library repo asking how to reopen tabs that still exist as TabState files but are no longer present in the WindowState file. I had wondered if this was possible as there is a maximum number of supported tabs that you can have open in Windows Notepad. I have not actually tested this but it would make sense since the number of tabs is stored in the WindowState file as a LEB128. 

Surprised someone actually hit this maximum or it is possible they hit some sort of corruption which left orphaned TabState files behind. I had written some functions into the library that provided some minimal functionality for modifying an existing WindowState or TabState file. I made use of some of them in my [POC Malware](https://ogmini.github.io/2025/02/07/GaslitPad-Release.html) but it did not mess with the WindowsState file. So most of it is untested and I haven't been able to think of a good use case for DFIR functions. 

The issue had me thinking though... Would it be useful from a standpoint of being able to visually show someone what those TabStates would look like in Windows Notepad? Being able to generate or recreate a WindowState file to display given TabState files wouldn't be incredibly difficult. I already have a function called called [WriteTabList](https://github.com/ogmini/Notepad-State-Library/blob/main/NotepadStateLibrary/NotepadStateLibrary/NPWindowState.cs) in the library that takes a list of byte arrays with each byte array representing the GUID of a TabState file. The function correctly sets the NumberOfTabs according to the list. 

Maybe I'll write something up real quick.
