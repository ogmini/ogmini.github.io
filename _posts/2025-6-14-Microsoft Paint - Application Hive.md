---
layout: post
title: Microsoft Paint - Application Hive
author: 'ogmini'
tags:
 - research
 - microsoft paint
---

Microsoft has been updating Paint, Snipping Tool, and Notepad at a pretty good pace recently. Mainly pushing AI... You can read more about some recent future updates at [https://blogs.windows.com/windows-insider/2025/05/22/paint-snipping-tool-and-notepad-updates-with-new-features-begin-rolling-out-to-windows-insiders/](https://blogs.windows.com/windows-insider/2025/05/22/paint-snipping-tool-and-notepad-updates-with-new-features-begin-rolling-out-to-windows-insiders/). All three of them are very similar in how they might store settings and other digital artifacts. This shouldn't be a surprise as I believe they are all Windows App SDK or UWP based. Yesterday's [post](https://ogmini.github.io/2025/06/13/Windows-Notepad-Revisiting-Application-Hive-Part-2.html), gave me a good reason to actually go take a quick look at Paint.

## User.dat 

The `User.dat` is found at `C:\Users\Reversing\AppData\Local\Packages\Microsoft.Paint_8wekyb3d8bbwe\SystemAppData\Helium` and can contain keys for:

- MRU
- TypedPaths 
- MountPoints2
- Recent Files

I'm not going to rehash information about the MRU/TypedPaths/MountPoints2. There are links in previous posts talking about them.

Recent Files are stored here under `\Software\Microsoft\Windows\CurrentVersion\Applets\Paint\Recent File List`. It also appears there is the possiblity of recovering deleted values of previous recent files. This needs more testing. 

![User.dat](/images/mspaint/userdat.png)

## UserClasses.dat

Again, the `UserClasses.dat` file contains Shellbags. Refer to previous [post](https://ogmini.github.io/2025/06/13/Windows-Notepad-Revisiting-Application-Hive-Part-2.html) for more details.

![UserClasses.dat](/images/mspaint/userclasses.png)

This is just a very quick peek and I need to do a lot more digging and testing. 