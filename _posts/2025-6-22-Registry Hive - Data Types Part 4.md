---
layout: post
title: Registry Hive - Data Types Part 4
author: 'ogmini'
tags:
 - Registryhive
---

Still stuck on the CompositeValue and this is a reminder that most of my posts are daily updates on what I'm working on. This is where I currently stand and I have a feeling the fix is easy and I'm just unsure on how to account for it within 010 Editor's Binary Templates.

The individual values contained within a CompositeValue are each  structured as:

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

The issue is the unkBytes or Unknown Bytes. They appear to be padding as they are all 0s. I do not know how to calculate the number of unkBytes. This might be trivial for someone with more experience so any help would be greatly appreciated. Some example test cases:

| TotalLength | ValueLength | KeyLength | Correct Padding | Start Position | Date Type | Notes |
| 52 | 12 | 13 | 4 | 4916 | String | String is 12 bytes and lines up. |
| 38 | 4 | 10 | 2 | 4972 | Int | Int is 4 bytes and lines up. |
| 46 | 16 | 8 | 2 | 5012 | GUID | GUID is 16 bytes and lines up. |
| 36 | 8 | 9 | 0 | 5060 | Float | Float is 8 bytes and lines up. |
| 46 | 15 | 12 | ??? | 5100 | Timespan | 15 for value length doesn't line up as a Timepsan is represented by a Int64 and is 8 bytes. |

![010 Editor](/images/registry/settings_composite2.png)

Astute readers may notice that nothing above tells us what the data type of each KeyValue is and as far as I can tell it doesn't appear to be in the binary file anywhere. For now, I'm ignoring it and maybe something will make itself known in the future or more confusion will result...
