---
layout: post
title: Researching RDCMan - Part 3
author: 'ogmini'
tags:
 - DFIR
 - RDCMan
---

Was poking around the Recent Virtual Group settings and it looks like I discovered a bug in the latest version (v3.1) of RDCMan. I've already [reported](https://learn.microsoft.com/en-us/answers/questions/2264786/rdcman-recent-servers-broken-in-3-1) it so hopefully it will get fixed. Would be a useful forensic artifact to have! It does work as expected on an older version of RDCMan that I still have (v2.93). So the below testing has been done on that version.  

### Recent Virtual Group
By default, this displays the last 10 servers that have had successful connections. You'll find this in the RDCMan.settings file under the recentlyUsed element. The list of servers is in most recently connected order. 

![Built In Groups](/images/RDCMan/builtingroups.png)

## References 
- [https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman)
