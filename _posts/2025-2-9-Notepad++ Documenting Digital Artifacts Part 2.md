---
layout: post
title: Notepad++ - Documenting Digital Artifacts Part 2 
author: 'ogmini'
tags:
 - DFIR
 - Notepad++
---

I think I'm done researching what digital artifacts can be retrieved from [Notepad++](https://notepad-plus-plus.org/). I've been able to confirm/validate the findings from [Forensafe](https://forensafe.com/blogs/windows_notepad++.html) and I will be providing more detailed information about them below. As I stated in [Part 1](https://ogmini.github.io/2025/02/08/Notepad++-Documenting-Digital-Artifacts.html), there is no real complication to the digital artifacts and everything is human readable with any text editor. In a future post, I will be pointing out how Windows Notepad and Notepad++ achieve similar functionality while storing the information differently.

## Location

In [Part 1](https://ogmini.github.io/2025/02/08/Notepad++-Documenting-Digital-Artifacts.html), I identified that the location of these digital artifacts will differ between the installed an portable versions of Notepad++.

| Version | XML Locations | Backup Location |
| --- | --- | --- |
| Installed | %appdata%\Notepad++ | %appdata%\Notepad++\backup |
| Portable | portable folder (Ex. D:\notepad) | backup folder in the portable folder (Ex. D:\notepad++\backup) |

## Recent History

Recently closed files are stored in the `config.xml` file under the "History" element which has "File" child elements for each recently closed file. Each "File" element has an attribute for filename which is just the path to the recently closed file.

## Sessions

Notepad++ has a `session.xml` and an associated `session.xml.inCaseOfCorruption.bak`. Everytime a new tab is opened or closed in Notepad++ it will update the `session.xml` and save the old one as the .bak file. This does leave a window of time for an investigator to see what the previous opened tab was in Notepad++.

As these are just xml files, the data contained within them is very human readable. I'm going to call out the most interesting "File" element attributes from a DFIR perspective:

| Key | Value Notes |
| --- | --- |
| filename | If filename is a filepath, the file is resident on a disk. If filename is just a phrase, the text only exists within Notepad++ as unsaved changes. |
| backupFilePath | If blank, there are no unsaved changes. If there is a path, this is the file that contains any unsaved changes. |
| originalFileLastModifTimestampHigh | dwHighDateTime part of the FILETIME structure [https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN)|
| originalFileLastModifTimestamp | dwLowDateTime part of the FILETIME structure [https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN)|

### Original File Last Modified Timestamp

FILETIME consists of two 32-bit values:

- dwLowDateTime: The lower 32 bits of the timestamp.
- dwHighDateTime: The upper 32 bits of the timestamp.

 Together these form a 64-bit timestamp representing the number of 100-nanosecond intervals since January 1, 1601 (UTC). Handy converter can be found at [https://www.epochconverter.com/ldap](https://www.epochconverter.com/ldap). I wasn't able to find a converter that you can give the LowDateTime and HighDateTime so I ended up coding one up. I'm hosting it as a HTML page on GitHub at [https://ogmini.github.io/FILETIME_Converter_Page/](https://ogmini.github.io/FILETIME_Converter_Page/).

 More details can be found at:

- [https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime?redirectedfrom=MSDN)
- [https://zetcode.com/gui/winapi/datetime/](https://zetcode.com/gui/winapi/datetime/)

## Backup

Notepad++ has a "backup" folder that contains the unsaved content from the tabs. One interesting behavior that I've noticed is that the unsaved content for tabs that are not files appears to stick around even after that tab has been closed and not saved. I need to explore this behavior more. On one of the systems that I use Notepad++ on I have some files dating back 3 months for tabs that are no longer present. Maybe these get flushed after a period of time? Could be useful from a DFIR perspective if these files stick around.

Each file in this folder might be referenced by a "File" element in the `session.xml` file. The filename itself has the following pattern:

[tab name]@[date]_[24H time]

Example: `new 2@2025-02-09_201043`

Tab is called "New 2" and was created/opened at 2/9/2025 8:10:43 PM. Note, that this doesn't need to match with the file's modified timestamp as that reflects when changes were made to the file.

These files are very easy to read as they are just straight text and can be opened by any text editor.
