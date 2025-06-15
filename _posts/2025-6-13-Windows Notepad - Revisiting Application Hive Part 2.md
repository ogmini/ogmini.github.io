---
layout: post
title: Windows Notepad - Revisiting Application Hive Part 2
author: 'ogmini'
tags:
 - research
 - windows-notepad 
---

Yesterday, [Chris Ray](https://www.linkedin.com/in/chris-ray-88175a21/) pointed me towards some more things they had observed in the `User.dat`. As he [states](https://www.linkedin.com/feed/update/urn:li:activity:7339071510768214016?commentUrn=urn%3Ali%3Acomment%3A%28activity%3A7339071510768214016%2C7339115765557542912%29&dashCommentUrn=urn%3Ali%3Afsd_comment%3A%287339115765557542912%2Curn%3Ali%3Aactivity%3A7339071510768214016%29), application registries do not care about the user disabling [app launch tracking](https://www.elevenforum.com/t/enable-or-disable-app-launch-tracking-in-windows-11.3727/). It helpfully ignores the "Start_TrackProgs" registry setting. This results in some more useful digital artifacts.  

## User.dat 

The `User.dat` is found at `%localappdata%\Packages\Microsoft.WindowsNotepad_8wekyb3d8bbwe\SystemAppData\Helium` and can contain keys for:

- TypedPaths 
- MountPoints2. 

Chris Ray has a great post at [https://www.cybertriage.com/blog/windows-registry-forensics-cheat-sheet-2025/](https://www.cybertriage.com/blog/windows-registry-forensics-cheat-sheet-2025/) highlighting areas of interest in the Windows Registry. The `User.dat` follows the structure of the `NTUSER.DAT` but is application specific for Windows Notepad. The `UserClasses.dat` follows the structure of the `UsrClass.dat` but is application specific for Windows Notepad. Granted, I would not expect to see the RunMRU or OfficeMRU keys in the `User.dat`. 

He has also posted about the Shellbags in the `UserClasses.dat` file at [https://www.cybertriage.com/blog/shellbags-forensic-analysis-2025/](https://www.cybertriage.com/blog/shellbags-forensic-analysis-2025/). Specifically using the Windows Notepad package as an example. Tools such as ShellBags Explorer/SBECmd can extract these digital artifacts. 

I'll have to play around with this to verify behaviour. From what quick testing I've done already, I can see that TypedPaths is populated. I've also started on a batch file for RECmd to pull these out for faster analysis. Thanks as always to [Andrew Rathbun](https://www.linkedin.com/in/andrewrathbun) for pointing out more ways to contribute.  

![TypedPaths](/images/windowsnotepad/typedpaths.png)

## settings.dat

RECmd and RegistryExplorer are currently unable to display the information from this file in a nice way. I took a peek at the various RegistryPlugins and I'm hoping to extend support to this file in the future. At the moment, if you run RECmd/Registry Explorer against the `settings.dat` you get the output below. If you know what you're looking at in the Value Data column it all makes sense.

![RECmd](/images/windowsnotepad/futureregistryplugin.png)

![Registry Explorer](/images/windowsnotepad/futureregistryplugin2.png)