---
layout: post
title: Windows Notepad - Revisiting Application Hive
author: 'ogmini'
tags:
 - research
 - windows notepad 
---

Let's keep this ball rolling! Two different things I'm still looking at in more detail to understand them as much as possible. The first was a reminder by [@M4shl3](https://github.com/M4shl3) to look at the `User.dat` who recently left a comment on yesterday's [post](https://ogmini.github.io/2025/06/11/Windows-Notepad-Find-Replace-Bing.html). The second is more keys in the `settings.dat` that I had somehow missed previously or are new and a datapoint that isn't so obvious depending on how you look at the file. 

## User.dat and UserClasses.dat

The `User.dat` and `UserClasses.dat` are found at `%localappdata%\Packages\Microsoft.WindowsNotepad_8wekyb3d8bbwe\SystemAppData\Helium`. If you open the `User.dat` and examine the keys at \Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32 we can see some familiar sights. Specifically MRU! Remember this is a seperate/different digital artifact from the RecentFiles. Much better writeups about MRU can be found at:

- [https://www.sans.org/blog/opensavemru-and-lastvisitedmru/](https://www.sans.org/blog/opensavemru-and-lastvisitedmru/)
- [https://www.magnetforensics.com/blog/what-is-mru-most-recently-used/](https://www.magnetforensics.com/blog/what-is-mru-most-recently-used/)

There are also some other keys of interest which don't appear to be used at the moment. It is also likely that I need to "use" Windows Notepad is a specific way to get them to do something. There are keys at \Software\Microsoft\OneDrive\Accounts and \Software\Microsoft\Spelling\Dictionaries. I would hazard a guess that Dictionaries is somehow related to the AutoCorrect dictionary capability in Windows Notepad. I'm just not sure how yet; but I haven't been able to get any values written to the keys.

The `UserClasses.dat` contains keys for Shellbags. Again, better writesup about ShellBags can be found at:

- [https://www.magnetforensics.com/blog/forensic-analysis-of-windows-shellbags/](https://www.magnetforensics.com/blog/forensic-analysis-of-windows-shellbags/)

## settings.dat

| KeyName | Type | Notes |
| --- | --- | --- |
| FirstSignIn | 0x5f5e10b  |  |
| WebAccountType | 0x5f5e104 |  |
| PrivacyTeachingTip | 0x5f5e10b | |
| FirstCowriterClick | 0x5f5e10b | |

I mentioned that there was a datapoint that I had overlooked previously which is funny because I had updated the [binary template](https://www.sweetscape.com/010editor/repository/templates/file_info.php?file=RegistryHive.bt&type=0&sort=) for 010 Editor that helps to highlight the Timestamp for keys. If you examine the `settings.dat` file using tools like Registry Editor, Registry Spy, or Registry Explorer they will not show this information in an obvious fashion. It is there, but you have to extract it out of the Data field itself. In the case of the screenshots below. We have a timestamp of 6/12/2025 14:31:45 that is repesented as a FILETIME by EC-F7-0E-B9-A6-DB-DB-01.  
 

010 Editor w/RegistryHive Binary Template    
![010 Editor](/images/windowsnotepad/regtimestamp_010.png)

Registry Explorer    
![Registry Explorer](/images/windowsnotepad/regtimestamp_regexp.png)

Registry Spy 
![Registry Spy](/images/windowsnotepad/regtimestamp_regspy.png)

I need to do more testing before I commit to understanding the behavior and importance of this timestamp. But, I will say that the Timestamp at least for the FirstSignIn does appear to line up with the time that I signed into my Microsoft account in Windows Notepad.

## Thoughts

It is very likely that some of the keys that I'm seeing are there for future features that are being rolled out on the Dev/Canary Channels for Windows 11. I'm still only doing research on the Release Channel. Microsoft had two recent posts about additions to Windows Notepad:

- [https://blogs.windows.com/windows-insider/2025/05/30/text-formatting-in-notepad-begin-rolling-out-to-windows-insiders/](https://blogs.windows.com/windows-insider/2025/05/30/text-formatting-in-notepad-begin-rolling-out-to-windows-insiders/)
- [https://blogs.windows.com/windows-insider/2025/05/22/paint-snipping-tool-and-notepad-updates-with-new-features-begin-rolling-out-to-windows-insiders/](https://blogs.windows.com/windows-insider/2025/05/22/paint-snipping-tool-and-notepad-updates-with-new-features-begin-rolling-out-to-windows-insiders/)

We're getting more AI and Markdown support. More posts to come as I continue to dig in.