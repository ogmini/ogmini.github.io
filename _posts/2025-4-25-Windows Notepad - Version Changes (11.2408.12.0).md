---
layout: post
title: Windows Notepad - Version Changes (11.2408.12.0)
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

Today, I'm looking at Windows Notepad 11.2409.9.0 and comparing it to the previous version 11.2408.12.0. This could be useful if you somehow come across an older version of Windows Notepad on a system that is being investigated. For more details refer to [https://github.com/ogmini/Notepad-State-Library](https://github.com/ogmini/Notepad-State-Library).

If you don't want to read the rest. __There are no changes from 11.2408.12.0 to 11.2409.9.0__.

## File Tabs

The first thing we'll look at are the Tabstate files for File Tabs. These are files that exist on a disk with saved or unsaved changes. Below is a table outlining if the artifact is present in the specific version and any notes.

| Data | 11.2402.22.0 | 11.2407.9.0 | 11.2408.12.0 | 11.2409.9.0 | Notes |
| --- | --- | --- | --- | --- | --- |
| File Hash | x | | | |This File Hash is for the file on disk. It does NOT account for any unsaved changes. |
| Option Count | x | x | x | x |This is 1 for 11.2402.22.0 and 2 for 11.2407.9.0+. It is still unknown what these are. |
| Saved File Content Length | x | x | / | / | Starting with 11.2408.12.0 this is only populated for File Tabs with unsaved changes.|
| Timestamp | x | x | / | / |Starting with 11.2408.12.0 this is only populated for File Tabs with unsaved changes.|
| Selection Start/End Index | x | x | | | Starting with 11.2408.12.0 this is no longer populated. |
| Content Length | x | x | / | / | Starting with 11.2408.12.0 this is only populated for File Tabs with unsaved changes.|
| Content | x | x | / | / | Starting with 11.2408.12.0 this is only populated for File Tabs with unsaved changes.|

No significant changes from 11.2408.12.0 to 11.2409.9.0.

## No File Tabs

No File Tabs are tabs in Notepad that have not been saved to disk. They only exist in the state files. Below is a table outlining if the artifact is present in the specific version and any notes.

| Data | 11.2402.22.0 | 11.2407.9.0 | 11.2408.12.0 | 11.2409.9.0 | Notes |
| --- | --- | --- | --- | --- | --- |
| Option Count | x | x | x | x| This is 1 for 11.2402.22.0 and 2 for 11.2407.9.0+. It is still unknown what these are. |

No significant changes from 11.2408.12.0 to 11.2409.9.0.

## Unsaved Buffer Chunks

If you were to examine the state files while Windows Notepad was still open and had unsaved changes. You would see all the unsaved changes. There are no changes in how these behave.

| Data | 11.2402.22.0 | 11.2407.9.0 | 11.2408.12.0 | 11.2409.9.0 | Notes |
| --- | --- | --- | --- | --- | --- |
| Unsaved Buffer Chunks | x | x | x | x | Identical |

No significant changes from 11.2408.12.0 to 11.2409.9.0.
