---
layout: post
title: Lifecycle of a Digital Photo on a Android Pixel 7 - Part 1
author: 'ogmini'
tags:
 - Android
 - Google-Photos
 - EXIF
 - Timestamps
---

Recent events have resulted in my interest and examination of photos taken by an Android phone. What is the lifecycle of a typical photo taken by an Android phone with the Google Photos applilcation setup to backup photos to the owner's Google account? What timestamps can we find? What are some potential indicators of timestomping or manipulation?

## EXT4 Timestamps

This is a bit of a continuation from a previous post - [https://ogmini.github.io/2025/09/20/Pixel-7-EXT4-Timestamps.html](https://ogmini.github.io/2025/09/20/Pixel-7-EXT4-Timestamps.html). When you take a photo it must be stored somewhere. On my testing Pixel 7, when I take a photo it is stored at `/storage/self/primary/DCIM/Camera` and the filenames follow a format of `PXL_YYYYMMDD_HHmmssfff`. This seems like a good place to start. Using adb, I run the stat command on an image I took to check out the MAC timestamps.

~~~ cmd
Access: 2025-09-17 12:34:23.012150822 -0400
Modify: 2025-09-17 12:34:27.656150824 -0400
Change: 2025-09-17 12:34:27.776150825 -0400
~~~

I pull the image down to check the EXIF metadata. Using ExifDataView from Nirsoft reports back:

| Property Group | Property Name | Value |
| --- | --- | --- |
| Image | DateTime | 2025:09:17 12:34:22 |
| Camera | DTOrig | 2025:09:17 12:34:22 |
| Camera | DTDigitized | 2025:09:17 12:34:22 |

exiftool reports back:

| Name | Value |
| --- | --- |
| Date/Time Original | 2025:09:17 12:34:22.921-04:00 |
| Create Date | 2025:09:17 12:34:22.921-04:00 |
| Modify Date | 2025:09:17 12:34:22.921-04:00 |

I need to dig into to understand the differences between the two tools and what they are reporting. I'm sure there are some intricacies to take not of here.

From what I can see from the above and my current understanding, the photo was taken at 9/17/2025 at 12:34:22.921. It was then accessed, modified, and changed at the times listed above. I'm interested to know why these timestamps and I'll have to dig deeper into EXT4 timestamp behaviour.

## Next Steps/Questions

Is there a way to tell when the photo was uploaded to Google Photos? Will the act of uploading change any of the EXT4 timestamps? How can I timestomp/alter the EXT4 timestamps?
