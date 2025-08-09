---
layout: post
title: Windows Snipping Tool - Part 1
author: 'ogmini'
tags:
 - Snipping-Tool
 - Registryhive
 - RegistryPlugins
---

Let us get into more detail about my previous [post](https://ogmini.github.io/2025/08/07/Poking-Windows-Snipping-Tool.html) about poking around Windows Snipping Tool. Today, I'll be focusing on two specific values being stored in the LocalState key in the `settings.dat` file which is located at `%localappdata%\Packages\Microsoft.ScreenSketch_8wekyb3d8bbwe\Settings`. The Windows Snipping tool has the option to automatically save any screenshots or recordings to a default location. This default location can be changed by the user. In the screenshot below, we can see that the screenshot location has been set to `C:\Users\User\Desktop\SnipFolderTest\30` and the recording location has been set to `C:\Users\User\Desktop\examine`.

![Settings](/images/snippingtool/settings-location.png)

This information is conveniently stored in the `settings.dat` file which is located at `%localappdata%\Packages\Microsoft.ScreenSketch_8wekyb3d8bbwe\Settings`. In this file, we find the following:

![Values](/images/snippingtool/FolderGUID.png)

| Value Name| Value Type | Notes |
| --- | --- | --- |
| AutoSaveRecordingsLocation | ReqUwpString | Persisted Storage Item GUID |
| AutoSaveScreenshotsLocation | ReqUwpString | Persisted Storage Item GUID |

For now, let us focus on the AutoSaveScreenshotsLocation which has a GUID of **{200818BD-3D5A-4986-AA9D-21F9854F3365}**.

Great, what do we do with this GUID! Next stop is the `UserClasses.dat` file located at `%localappdata%\Packages\Microsoft.ScreenSketch_8wekyb3d8bbwe\SystemAppData\Helium`. Chris Ray over at CyberTriage has a good [article](https://www.cybertriage.com/blog/windows-registry-forensics-2025/) about Windows Registry Forensics and calls out these application specific (MSIX) based registry hives. We are about to see an example of why they are important and should not be forgotten.

![UserClasses.dat](/images/snippingtool/FolderPath.png)

What do we find at the following key `Local Settings\Software\Microsoft\Windows\CurrentVersion\AppModel\SystemAppData\Microsoft.ScreenSketch_8wekyb3d8bbwe\PersistedStorageItemTable\ManagedByApp\`?

A key for our GUID **{200818BD-3D5A-4986-AA9D-21F9854F3365}** and the actual path to the folder in the FilePath value as `\\?\Volume{EEF0A800-0E12-40F9-8567-78259FA1DAF6}\Users\User\Desktop\SnipFolderTest\30`.

Using mountvol we can find out what Volume Name corresponds to that Volume GUID and translate that FilePath to `C:\Users\User\Desktop\SnipFolderTest\30`.

![mountvol](/images/snippingtool/mountvol.png)

Some readers may have already noticed that there were many Persisted Storage Item GUIDs located in the `UserClasses.dat` and the program only needs a maximum of two for the two autosave locations. It appears to keep the past/previous autosave locations. I was trying to figure out how many historical records it keeps; but I got bored after 33 different locations. I am interested to see if there is a maximum or what if anything causes the older records to get flushed.

This could be a useful artifact to poke around as it could point out folder locations that an investigator might want to examine. For example, was someone automatically saving screenshots/recordings of sensitive information to a USB Key?

Persisted Storage Item GUID -> FilePath Volume GUID -> MountPoint2 Registry

I'm going to continue looking at Snipping Tool much like I did with Windows Notepad. This information will ultimately be compiled into a more formal writeup. I want to document all the values in the `settings.dat` file and their potential significance.
