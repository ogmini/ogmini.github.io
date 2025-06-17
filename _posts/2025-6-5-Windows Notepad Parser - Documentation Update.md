---
layout: post
title: Windows Notepad Parser - Documentation Update
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
---

No major changes to Windows Notepad Parser. Yogesh Khatri helpfully pointed out that I had not actually documented the command line arguments and it appeared that the program would only work on a live machine. I thank him for opening an [issue](https://github.com/ogmini/Notepad-State-Library/issues/1) to point this out. 

I've since updated the documentation to specifically call out the command line arguments. I've included them below for ease:

```
 -t, --tabstatelocation       Tab State Folder Location. Default value is the system location.

 -w, --windowstatelocation    Window State Folder Location. Default value is the system location.

 -o, --outputlocation         Output Folder Location for CSV files. Default location is same folder as program.

 --help                       Display this help screen.

 --version                    Display version information.
```

I'll be sure to include a README in future compiled releases. 



