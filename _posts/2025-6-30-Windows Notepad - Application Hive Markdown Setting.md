---
layout: post
title: Windows Notepad - Application Hive Markdown Setting
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

With the introduction of Markdown support we have a new setting in the `settings.dat` file to enable/disable the Formatting options. Using Registry Explorer we can take a look and see it:

![FormattingEnabled](/images/windowsnotepad/FormattingEnabled.png)

Using 010 Editor and the template.

![010FormattingEnabled](/images/windowsnotepad/010FormattingEnabled.png)

| KeyName | Type | Notes |
| --- | --- | --- |
| Formatting Enabled | 0x5f5e10b | 0 = Off / 1 = On |

More to come as I get more time to dig in.
