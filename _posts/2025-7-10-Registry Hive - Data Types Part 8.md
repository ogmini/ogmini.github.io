---
layout: post
title: Registry Hive - Data Types Part 8
author: 'ogmini'
tags:
 - Registryhive
---

A good reminder that we always, always need to test/verify. Looks like my assumption from a previous [post](https://ogmini.github.io/2025/07/07/Registry-Hive-Data-Types-Part-7.html) about the identifier for each CompositeValue types was wrong! They do not ascend in order.

I tested with my simple test application and got the following results:

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
| 33 | ArrayDateTime Offset |
| 34 | ArrayTimespan |
| 35 | ArrayGUID |
| 36 | ArrayPoint |
| 37 | ArraySize |
| 38 | ArrayRect |

It does not appear that you can have nested CompositeValues which I would have expected to see with a Type of 13. Instead we have a jump from 12 to 14. There is also a jump from 31 to 33; but that could be accounted for the missing Array of CompositeValues.

Kind of scary, I'm getting very comfortable looking at the raw binary data of the settings.dat without any highlighting or templates. Still don't have my binary template working correctly yet. Screenshot below of the raw binary with the some of the Types highlighted.

![Raw Binary Highlight Types](/images/registry/rawbinarycomposite.png)