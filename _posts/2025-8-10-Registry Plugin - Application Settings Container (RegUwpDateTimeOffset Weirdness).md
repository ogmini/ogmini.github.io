---
layout: post
title: Application Settings Container - RegUwpDateTimeOffset Weirdness?
author: 'ogmini'
tags:
 - Registryhive
 - RegistryPlugins
---

Been bouncing back and forth this weekend with [@reece394](https://github.com/reece394) working on the ApplicationSettingsContainer. He has also been putting the plugin to use and came across an oddity relating to the RegUwpDateTimeOffset. You can follow and feel free to join the conversation at [https://github.com/EricZimmerman/RegistryPlugins/pull/68](https://github.com/EricZimmerman/RegistryPlugins/pull/68).

We have determined that the RegUwpDateTimeOffset value is still an Int64 that could either represent:

1. A Windows Filetime that counts the number of 100-nanosecond intervals since January 1, 1601 (UTC)
    - [https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime](https://learn.microsoft.com/en-us/windows/win32/api/minwinbase/ns-minwinbase-filetime)
2. A DateTime.Ticks that counts the number of 100-nanosecond intervals since January 1, 0001
    - [https://learn.microsoft.com/en-us/dotnet/api/system.datetime.ticks?view=net-9.0](https://learn.microsoft.com/en-us/dotnet/api/system.datetime.ticks?view=net-9.0)

Consequently, the registry plugin at the moment will just be displaying the Int64 with a note about the above. This leaves the human to determine which type it is and to convert appropriately. This should be a very simple task for a human as the different integers are very far apart. Generally, Windows FILETIME will start with a 1 and DateTime.Ticks will start with a 6. There are many tools/sites that can be used to convert.

[https://ogmini.github.io/FILETIME_Converter_Page/](https://ogmini.github.io/FILETIME_Converter_Page/) - Windows FILETIME Converter
[https://www.datetimetoticks-converter.com/](https://www.datetimetoticks-converter.com/) - DateTime.Ticks Converter
