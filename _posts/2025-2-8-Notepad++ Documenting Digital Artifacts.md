---
layout: post
title: Notepad++ - Documenting Digital Artifacts    
author: 'ogmini'
tags:
 - DFIR
---

Stemming from my research into Windows Notepad, I think it would be fun to take a look at Notepad++ and maybe other text editors like Visual Studio Code to see what kind of digital artifacts we can uncover. Personally, I use Notepad++ and I'm sure it is a very popular text editor for many other people. I'm sure others have already looked and I've found information from [Forensafe](https://forensafe.com/blogs/windows_notepad++.html) mainly showing how their tool can recover information. They didn't go into many details on the page and I think it would be worthwhile to document that more fully in the open for all to see. It is also possible that changes have been made to Notepad++ since the article. 

I'd also like to maybe extend [GaslitPad](https://ogmini.github.io/2025/02/07/GaslitPad-Release.html) to possibly target Notepad++. 

## Known/Documented Details

Forensafe states that digital artifacts related to Notepad++ can be found in the following locations:

- %appdata%\Notepad++
- %appdata%\Notepad++\backup

I have confirmed that this is true for the Installed version of Notepad++ v8.7.7 downloaded from [https://notepad-plus-plus.org/downloads/](https://notepad-plus-plus.org/downloads/). This is not the case for the portable versions of Notepad++ v8.7.7 which instead have:

- portable folder (Ex. D:\notepad++)
- backup folder in the portable folder (Ex. D:\notepad++\backup)

 The backup folder contains text files of any unsaved tabs and the file name and file types contain useful information tying them back to the original tabs. The main folder contains a bunch of XML files with the most interesting being the session.xml and a backup which appears to keep information about the various open tabs/files. It is interesting to compare/contrast how Windows Notepad and Notepad++ handle saving states and unsaved changes. One interesting thing is that Notepad++ doesn't do the unsaved buffer chunks like Windows Notepad and instead just constantly updates the file in the backup folder with any changes. 

Just from a quick 5 minute glance, this doesn't feel like it will be as interesting to dig into as Notepad++ uses plain text or xml files. There are no binary files to deciper as with Windows Notepad. Still important to document and understand though!
