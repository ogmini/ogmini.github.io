---
layout: post
title: Volatility3 - Windows 11 24H2 Memory Dump issues?
author: 'ogmini'
tags:
 - DFIR
 - Volatility
 - Memory Forensics
---

Had a little bit of time today to start an attempt at using Volatility to look at Windows Notepad. Sadly, I immediately encountered some issues and went into troubleshooting mode. I used both FTK Imager and DumpIt to obtain memory dumps from my test Windows 11 24H2 26100.3775 install just to make sure it wasn't an issue with the tool I was using. I also downloaded an older Windows 11 sample memory dump from [https://www.osforensics.com/tools/volatility-workbench.html](https://www.osforensics.com/tools/volatility-workbench.html). This loaded up fine in Volatility3 and I was able to examine it as expected. The windows.info plugin provided the information below:

![Sample](/images/volatilityerrors/sampledump.png)

Running windows.info on my obtained memory dump gave initially encouraging information. Correctly reporting back the Major/Minor version. 

![Mine](/images/volatilityerrors/mydump.png)

Unfortunately, upon running windows.pslist I got the following message:

![Error Message](/images/volatilityerrors/errmsg.png)

> Volatility experienced a symbol-related issue:  
> symbol_table_name1!_MM_SESSION_SPACE: Enumeration not found in symbol_table_name1 table: _MM_SESSION_SPACE
>
>        * An invalid symbol table
>        * A plugin requesting a bad symbol
>        * A plugin requesting a symbol from the wrong table
>
>No further results will be produced

As you can see from the screenshots, I'm using Volatility 3 2.11.0 and I used the symbol pack for Windows from [https://github.com/volatilityfoundation/volatility3?tab=readme-ov-file#symbol-tables](https://github.com/volatilityfoundation/volatility3?tab=readme-ov-file#symbol-tables). That is as far as I got today, I'll probably open up an issue on the GitHub. I'm pretty sure I'm not missing anything. 