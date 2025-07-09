---
layout: post
title: Windows Notepad - Forced Save on Detecting Manipulation?
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

Came across an interesting behavior while doing some testing. Initially, I wasn't able to replicate it. Early on in I would sometimes see the creation of a guid.bin.tmp file in the TabState folder. At the time I had brushed it off as it would "resolve" by deleting itself and nothing seemed to break. I theorized that it might be related to file locks and Windows Notepad trying to protect the integrity of the TabState files.

Windows Notepad will now force you to decide between saving your open tabs when you close Windows Notepad if a .tmp file was ever created during the current session. I'm pretty sure this behavior didn't exist in previous version. Just to clarify, the forced saving I believe is new as I had noticed the creation of .tmp files in the past but not the prompt to save. I would need to regression test to verify. Some sort of security? The .tmp files are the same format as the normal TabState files but they contain the changes made during the "lock" and are ultimately rolled back into the normal bin file at the next chance.  

![Force Save](/images/windowsnotepad/forcedsave.png)

## Replication

If you want to replicate this behavior you can follow the steps below. Note this isn't exactly consistent as there is some very short timing required.

1. Open Windows Notepad and have a Tab with some Markdown text in it and it should be in the unsaved state.
2. Open the TabState file in 010 Editor.
3. Type a letter into the Windows Notepad Tab.
4. Quickly go back to 010 Editor and click on a cell. You will know if you did this right if 010 Editor doesn't prompt you to update the contents.
5. You should now see a guid.bin.tmp file.
6. Making another change in the Windows Notepad Tab will cause the deletion of the guid.bin.tmp file.

![GUID BIN TMP Gif](/images/windowsnotepad/bintmp.gif)
