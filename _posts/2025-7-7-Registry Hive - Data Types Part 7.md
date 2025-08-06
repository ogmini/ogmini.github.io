---
layout: post
title: Registry Hive - Data Types Part 7
author: 'ogmini'
tags:
 - Registryhive
---

> NOTE
> This has been superseded by [https://ogmini.github.io/2025/07/10/Registry-Hive-Data-Types-Part-8.html](https://ogmini.github.io/2025/07/10/Registry-Hive-Data-Types-Part-8.html)

Continuation from yesterday's [post](https://ogmini.github.io/2025/07/06/Registry-Hive-Data-Types-Part-6.html). Still haven't figured it all out. Making progress on the DataTypes though while I take a break from trying to decipher the other parts of the CompositeValues. I have some more theories to test out.  

The DataTypes that I've tested/verified so far:

| Int | DataType |
| --- | --- |
| 4 | Int |
| 8 | Float |
| 11 | Bool |
| 12 | String |
| 14 | DateTime Offset |
| 15 | Timespan |
| 16 | GUID |

Judging by patterns, these should follow the others and result in the following table:

| Int | DataType |
| --- | --- |
| 1 | Byte |
| 2 | Int16 |
| 3 | uInt16|
| 4 | Int32 |
| 5 | uInt32 |
| 6 | Int64 |
| 7 | uInt64 |
| 8 | Single |
| 9 | Double |
| 10 | Char |
| 11 | Bool |
| 12 | String |
| 13 | CompositeValue (Nest Composite Values?) |
| 14 | DateTime Offset |
| 15 | Timespan |
| 16 | GUID |
| 17 | Point |
| 18 | Size |
| 19 | Rect |
| 20 | ArrayByte |
| 21 | ArrayInt16 |
| 22 | ArrayuInt16|
| 23 | ArrayInt32 |
| 24 | ArrayuInt32 |
| 25 | ArrayInt64 |
| 26 | ArrayuInt64 |
| 27 | ArraySingle |
| 28| ArrayDouble |
| 29 | ArrayChar |
| 30 | ArrayBool |
| 31 | ArrayString |
| 32 | ArrayDateTime Offset |
| 33 | ArrayTimespan |
| 34 | ArrayGUID |
| 35 | ArrayPoint |
| 36 | ArraySize |
| 37 | ArrayRect |
