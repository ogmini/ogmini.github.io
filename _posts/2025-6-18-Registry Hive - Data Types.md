---
layout: post
title: Registry Hive - Data Types
author: 'ogmini'
tags:
 - Registryhive
---

While continuing to research the `settings.dat` file, I came across a Github repo specifically for editting `settings.dat` files related to UWP applications. Perusing the code revealed something interesting related the the Data Types I had spoken about yesterday. Previously, I had known of 4 from looking at Windows Notepad. The code actually outlines 36! [https://github.com/ADeltaX/UWPSettingsEditor/blob/master/src/UWPSettingsEditor/Enums/DataTypeEnum.cs](https://github.com/ADeltaX/UWPSettingsEditor/blob/master/src/UWPSettingsEditor/Enums/DataTypeEnum.cs).

The repo does not state where this information comes from or provide much in the way of documentation. The 4 that I know about do line up correctly.

This will require some testing/validation and my plan is to write a simple UWP application that leverages the `settings.dat` to save/edit some test data. This should mimic how other UWP applications interact.

[https://learn.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localsettings?view=winrt-26100](https://learn.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localsettings?view=winrt-26100)
[https://blogs.windows.com/windowsdeveloper/2016/05/10/getting-started-storing-app-data-locally/#_Local](https://blogs.windows.com/windowsdeveloper/2016/05/10/getting-started-storing-app-data-locally/#_Local)
