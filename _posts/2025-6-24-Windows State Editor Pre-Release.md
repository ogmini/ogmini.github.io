---
layout: post
title: Windows Notepad - Windows State Editor Pre-Release
author: 'ogmini'
tags:
 - Windows-Notepad
---

So yes, ended up quickly slamming out a not thoroughly tested pre-release of Window State Editor. You can find the application at [https://github.com/ogmini/Notepad-State-Library/releases/tag/WindowStateEditor](https://github.com/ogmini/Notepad-State-Library/releases/tag/WindowStateEditor).

No guardrails to stop you from breaking everything. Allows you to edit the list of tabs and specify the active tab. As the release notes state:

Do not run this while Windows Notepad is open. You must provide a space separated list of GUIDs. Example:

Writes three Tabs to the Window State file.

``` cmd
WindowStateEditor.exe -t f0607dc3-5345-470c-8ce6-8a49fe80ca55 d7511898-2c9a-4a2a-88a8-2c92305333f7 606db564-c71c-4dc7-a295-591242cf59a2
```

Writes three Tabs to the Window State file and sets the Active Tab to the 2nd in the list.

``` cmd
WindowStateEditor.exe -t f0607dc3-5345-470c-8ce6-8a49fe80ca55 d7511898-2c9a-4a2a-88a8-2c92305333f7 606db564-c71c-4dc7-a295-591242cf59a2 -a 1
```

Maybe someone will find it useful for some purpose. The actual library was also updated to support this ability so that can also be used in other applications. I did have the intention of the library having enough functionality to be able to write valid TabState and WindowState files from scratch.
