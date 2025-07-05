---
layout: post
title: Registry Hive - Data Types Part 5
author: 'ogmini'
tags:
 - Registryhive
---

Unsure, if I'm making progress on the CompositeValues. I'm really stuck trying to figure out the structure. Last I left off I had the following theory:

```c
typedef struct () {
    int TotalLength <fgcolor=cWhite, bgcolor=cDkAqua>;
    int ValueLength <fgcolor=cWhite, bgcolor=cDkRed>;
    int KeyLength <fgcolor=cWhite, bgcolor=cDkGreen>;
    wchar_t KeyName[] <fgcolor=cWhite, bgcolor=cDkYellow>;
    byte KeyValue[ValueLength] <fgcolor=cWhite, bgcolor=cDkBlue>;
    
    byte unkBytes[TotalLength - ((ValueLength + KeyLength)*2)]; //padding??? This is wrong
} CompVal;
```
I'm still baffled that there is seemingly nothing telling us about the data type of the composite value. How would you know how to interpret the values? Prior to all the values in the array, there are 4 unknown bytes and I've noticed that the first byte does appear to change in a pattern. I haven't been able to fully discern what that pattern is. Is it important? During some testing with creating a composite value of multiple strings, it does appear to change as the string gets longer. 

Alternatively, am I overthinking this somehow and adding in some unneeded complexity? Maybe the application itself just try parses until it figures it out? One does argue that the program should know what data type it stored in the key since it initially wrote the value. 

