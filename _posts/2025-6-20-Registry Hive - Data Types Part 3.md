---
layout: post
title: Registry Hive - Data Types Part 3
author: 'ogmini'
tags:
 - Registryhive
---

And we're back online! I have a really simple program to help test and reverse engineer the `settings.dat` file. Helps me narrow down changes as I'm able to control the inputs by altering values and such. Quick snippet below for those interested. It isn't anything fancy. 

```csharp
Windows.Storage.ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;

localSettings.Values["AppGUID"] = Guid.NewGuid(); //GUID
localSettings.Values["MyName"] = "supercalifragilisticexpialidocious"; //String
localSettings.Values["MiddleInitial"] = 'V'; //character
localSettings.Values["IsTall"] = false; //Bool
localSettings.Values["HeightFloat"] = 5.11f; //float
localSettings.Values["HeightDouble"] = 5.11d; //double
// SNIPPED ETC
localSettings.Values["PointValue"] = new Point(5, 5); //Point
localSettings.Values["SizeValue"] = new Size(5, 5); //Size
localSettings.Values["RectValue"] = new Rect(5, 5, 5, 5); //Rectangle

Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();

composite["compStringVal"] = "FINDME";
composite["compIntVal"] = (int)8675309;
composite["compGUID"] = Guid.NewGuid();
localSettings.Values["CompositeSettings"] = composite;

indows.Storage.ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
roamingSettings.Values["roamMyName"] = "Rover12";
```

I'm happy to say that I have confirmed that there are 36 data types. 18 of them are arrays of the other 18. 

| DataType | Identifier | 
| --- | --- |
|RegUwpByte | 0x5f5e_101 / 100000001
|RegUwpInt16 | 0x5f5e_102 / 100000002
|RegUwpUint16 | 0x5f5e_103 / 100000003
|RegUwpInt32 | 0x5f5e_104 / 100000004
|RegUwpUint32 | 0x5f5e_105 / 100000005
|RegUwpInt64 | 0x5f5e_106 / 100000006
|RegUwpUint64 | 0x5f5e_107 / 100000007
|RegUwpSingle | 0x5f5e_108 / 100000008
|RegUwpDouble | 0x5f5e_109 / 100000009
|RegUwpChar | 0x5f5e_10A / 100000010
|RegUwpBoolean | 0x5f5e_10B / 100000011
|RegUwpString | 0x5f5e_10C / 100000012
|RegUwpCompositeValue | 0x5f5e_10D / 100000013
|RegUwpDateTimeOffset | 0x5f5e_10E / 100000014
|RegUwpTimeSpan | 0x5f5e_10F / 100000015
|RegUwpGuid | 0x5f5e_110 / 100000016
|RegUwpPoint | 0x5f5e_111 / 100000017
|RegUwpSize | 0x5f5e_112 / 100000018
|RegUwpRect | 0x5f5e_113 / 100000019
|RegUwpArrayByte | 0x5f5e_114 / 100000020
|RegUwpArrayInt16 | 0x5f5e_115 / 100000021
|RegUwpArrayUint16 | 0x5f5e_116 / 100000022
|RegUwpArrayInt32 | 0x5f5e_117 / 100000023
|RegUwpArrayUint32 | 0x5f5e_118 / 100000024
|RegUwpArrayInt64 | 0x5f5e_119 / 100000025
|RegUwpArrayUint64 | 0x5f5e_11A / 100000026
|RegUwpArraySingle | 0x5f5e_11B / 100000027
|RegUwpArrayDouble | 0x5f5e_11C / 100000028
|RegUwpArrayChar16 | 0x5f5e_11D / 100000029
|RegUwpArrayBoolean | 0x5f5e_11E / 100000030
|RegUwpArrayString | 0x5f5e_11F / 100000031
|RegUwpArrayDateTimeOffset | 0x5f5e_120 / 100000032
|RegUwpArrayTimeSpan | 0x5f5e_121 / 100000033
|RegUwpArrayGuid | 0x5f5e_122 / 100000034
|RegUwpArrayPoint | 0x5f5e_123 / 100000035
|RegUwpArraySize | 0x5f5e_124 / 100000036
|RegUwpArrayRect | 0x5f5e_125 / 100000037

![010 Editor](/images/registry/settings_datatypes.png)

I have not completely figured out the format since I'm stuck on two things:

1. The structure of the composite value subkeys
2. Finish testing/clean up code for the array values 

Just need to keep plugging away at it. Just knee deep in staring at a hex editor... For the structure of the composite value keys, I can't figure out how the various subkey values are marked. How do I tell if a value is an int, string, etc. 

![010 Editor](/images/registry/settings_composite.png)

