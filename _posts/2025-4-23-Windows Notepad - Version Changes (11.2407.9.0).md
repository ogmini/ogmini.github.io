---
layout: post
title: Windows Notepad - Version Changes (11.2407.9.0)
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

During my initial research into Windows Notepad, I had noticed changes to the format of the state files and what was being stored or not stored in them. The initial version that I had torn apart was 11.2402.22.0. This post is the first part in going back to the previous versions and documenting changes. This could be useful if you somehow come across an older version of Windows Notepad on a system that is being investigated. For more details refer to [https://github.com/ogmini/Notepad-State-Library](https://github.com/ogmini/Notepad-State-Library). I will be documenting the changes from 11.2402.22.0 to 11.2407.9.0. Unfortunately, I have been unable to track down the installer for 11.2404.10.0 and I don't seem to have a VM Snapshot with that version. If anyone happens to have that version, please reach out!

## File Tabs

The first thing we'll look at are the Tabstate files for File Tabs. These are files that exist on a disk with saved or unsaved changes. Below is a table outlining if the artifact is present in the specific version and any notes.

| Data | 11.2402.22.0 | 11.2407.9.0 | Notes |
| --- | --- | --- | --- |
| File Hash | x | | This File Hash is for the file on disk. It does NOT account for any unsaved changes. |
| Option Count | x | x | This is 1 for 11.2402.22.0 and 2 for 11.2407.9.0. It is still unknown what these are. |

![File Hash Stored](/images/11.2402.22.0/FileHashStored.png)

![Option Count](/images/11.2402.22.0/OptionCount.png)

## No File Tabs

No File Tabs are tabs in Notepad that have not been saved to disk. They only exist in the state files. Below is a table outlining if the artifact is present in the specific version and any notes.

| Data | 11.2402.22.0 | 11.2407.9.0 | Notes |
| --- | --- | --- | --- |
| Option Count | x | x | This is 1 for 11.2402.22.0 and 2 for 11.2407.9.0. It is still unknown what these are. |

## Unsaved Buffer Chunks

If you were to examine the state files while Windows Notepad was still open and had unsaved changes. You would see all the unsaved changes. There are no changes in how these behave.

| Data | 11.2402.22.0 | 11.2407.9.0 | Notes |
| --- | --- | --- | --- |
| Unsaved Buffer Chunks | x | x | Identical |
