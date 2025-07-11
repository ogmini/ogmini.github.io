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

Snippet of the C# Code to create this `settings.dat` file.

~~~ c#
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();

composite["compByte"] = (byte)5;
composite["compInt16"] = (Int16)5;
composite["compuInt16"] = (UInt16)5;
composite["compIn32"] = (Int32)5;
composite["compuInt32"] = (UInt32)5;
composite["compInt64"] = (Int64)5;
composite["compuIn64"] = (UInt64)5;
composite["compSingle"] = (Single)5.11f;
composite["compDouble"] = 5.11d;
composite["compChar"] = 'a';
composite["compBool"] = true;
composite["compString"] = "This is my composite value string";
composite["compTimeOffset"] = DateTimeOffset.Now; 
composite["compTimeSpan"] = TimeSpan.FromHours(1);
composite["compGUID"] = Guid.NewGuid();
composite["compPoint"] = new Point(5, 5); 
composite["compSize"] = new Size(5, 5); 
composite["compRect"] = new Rect(5, 5, 5, 5);
composite["compArrayByte"] = new byte[] { 0x01, 0x02, 0xA5, 0xFF };
composite["compArrayInt16"] = new Int16[] { 100, 200, -300 };
composite["compArrayuInt16"] = new UInt16[] { 0, 100, 200 };
composite["compArrayIn32"] = new Int32[] { 100, 200, -300 };
composite["compArrayuInt32"] = new UInt32[] { 0, 100, 200 };
composite["compArrayInt64"] = new Int64[] { 100, 200, -300 };
composite["compArrayuIn64"] = new UInt64[] { 0, 100, 200 };
composite["compArraySingle"] = new Single[] { 1.23f, 4.56f, -7.89f };
composite["compArrayDouble"] = new double[] { 1.234567, 8.9101112, -3.14159 };
composite["compArrayChar"] = new char[] { 'A', 'b', '3', '!' };
composite["compArrayBool"] = new bool[] { true, false, false, true };
composite["compArrayString"] = new string[] { "Hello", "World", "C#", "Array" };
composite["compArrayTimeOffset"] = new DateTimeOffset[]
    {
        DateTimeOffset.Now,
        DateTimeOffset.UtcNow,
        new DateTimeOffset(2025, 7, 10, 12, 0, 0, TimeSpan.FromHours(-5))
    };
composite["compArrayTimeSpan"] = new TimeSpan[]
    {
        TimeSpan.Zero,
        TimeSpan.FromMinutes(90),
        new TimeSpan(1, 2, 30) // 1 hour, 2 minutes, 30 seconds
    };
composite["compArrayGUID"] = new Guid[]
    {
        Guid.NewGuid(),
        Guid.NewGuid(),
        new Guid("12345678-1234-1234-1234-123456789abc")
    };
composite["compArrayPoint"] = new Point[]
    {
        new Point(0, 0),
        new Point(10, 20),
        new Point(-5, 15)
    };
composite["compArraySize"] = new Size[]
    {
        new Size(100, 200),
        new Size(50, 50),
        new Size(0, 0)
    };
composite["compArrayRect"] = new Rect[]
    {
        new Rect(0, 0, 100, 200),
        new Rect(10, 10, 50, 50),
        new Rect(new Point(5, 5), new Size(20, 40))
    };

localSettings.Values["CompositeSettings"] = composite;
~~~
