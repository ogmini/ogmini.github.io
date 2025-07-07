---
layout: post
title: Registry Hive - Data Types Part 6
author: 'ogmini'
tags:
 - Registryhive
---

OK, feeling more confident after today's session. What I previously was calling a ValueLength seems to actually be the part that is telling me the data type. Below is this current work in progress. It still isn't fully complete/right. Namely, I'm unsure how to correctly calculate the DataLength.  

```c
typedef struct (int DataLength) {
    int TotalLength <fgcolor=cWhite, bgcolor=cDkAqua>;
    int DataType <fgcolor=cWhite, bgcolor=cDkRed>;
    int KeyLength <fgcolor=cWhite, bgcolor=cDkGreen>;
    wchar_t KeyName[] <fgcolor=cWhite, bgcolor=cDkYellow>;
    byte KeyValue[DataLength - 12 - ((KeyLength + 1) * 2) ] <fgcolor=cWhite, bgcolor=cDkBlue>;

} CompVal;
```

If it isn't obvious, I limit myself to about 45 minutes a day to work on these projects. This leaves me 15 minutes to write up the post. 

The DataTypes that I've tested so far:

| Int | DataType |
| --- | --- |
| 4 | Int |
| 12 | String |
| 16 | GUID |

I feel so tantalizingly close.