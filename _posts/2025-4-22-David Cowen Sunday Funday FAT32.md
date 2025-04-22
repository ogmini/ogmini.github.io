---
layout: post
title: David Cowen Sunday Funday Challenge - FAT32 Access Date?!
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

![FAT32 Party](/images/memes/FAT32_Party.png)

Another David Cowen Sunday Funday challenge posted at his [blog](https://www.hecfblog.com/2025/04/daily-blog-814-sunday-funday-42025.html) and it is about FAT32 Timestamps and the non-existence of the accessed timestamp. 

## Challenge

FAT32 does not store a time stamp for access dates, it only records the date. However many tools have or have in the past actually treated the zero time entry as a real time entry and adjusted it for time zones. Test your favorite tools such as , ftk imager, xways, axiom, encase, autopsy your choice but you must submit at least two and show if they are correctly handling FAT32 timestamps.

## Test Setup

I don't have access to any of the commercial tools so I'll be doing testing using:

- FTK Imager 4.7.3.81 
- Autopsy 4.22.1
- PowerShell

Decided to have a little fun with this one and I'll be examining the timestamps on the mp4 recordings from a Wyze Cam OG. The various Wyze Cameras support the use of microSD cards to locally record footage. I [reformatted](https://support.wyze.com/hc/en-us/articles/360031488091-How-to-Format-your-microSD-Card) the card to FAT32 before inserting it into the camera. After letting it record some footage, I pulled the card and made a forensically sound E01 image using FTK Imager. I ensured that the card was write blocked by using the hardware switch on my SD Card adaptor. 

## Testing

After acquiring the image, I opened the image and examined the `\record\20250422\11\39.mp4` file for timestamps. In the case of PowerShell, I mounted the image using FTK Imager. 

### FTK Imager

![FTK](/images/fat32timestamps/FTK_Imager.png)

FTK Imager only reports back Date Created and Modified. There is no Date Accessed shown. Contrast this to the timestamps reported for a NTFS Filesystem file seen below. We have Date Created, Modified, and Accessed and other NTFS Information below the DOS Attributes.

![FTK NTFS](/images/fat32timestamps/FTK_Imager_NTFS.png)

### Autopsy

![Autopsy](/images/fat32timestamps/Autopsy.png)

![Autopsy_TSK](/images/fat32timestamps/Autopsy_TSK.png)

Autopsy displays the Accessed and Changed timestamps as 0000-00-00 00:00:00. Autopsy is built upon TSK and we can see the same output from istat where it shows the Accessed timestamp as 0000-00-00 00:00:00. One important thing to note is that Autopsy has you set the Timezone of the Data Source at time of ingestion. That is why we are seeing EDT and not because it is coming from the Filesystem information. In the screenshots below, we are seeing the timestamp information for a file on a NTFS filesystem. 

![Autopsy NTFS](/images/fat32timestamps/Autopsy_NTFS.png)

![Autopsy_TSK NTFS](/images/fat32timestamps/Autopsy_TSK_NTFS.png)

### Powershell

![PowerShell](/images/fat32timestamps/Powershell.png)

Better be careful if you are using PowerShell to examine timestamps on FAT32 filesystems! Using the following command:

> Get-Item filename | Select-Object Name, CreationTime, LastAccessTime, LastWriteTime

 It reports back a access date and time of 12/31/1979 11:00:00 PM. For completions sake, below is a screenshot of running the same PowerShell command on a file in a NTFS filesystem. 

![PowerShell NTFS](/images/fat32timestamps/Powershell_NTFS.png)

## Conclusion

Understanding how FAT32 works and how it stores or doesn't store timestamps is still an important piece of knowledge. Many IOT devices still use the FAT32 or exFAT filesystem which does store the accessed timestamp. Just in my example, if you found a stack of microSD cards with footage that had been pulled from Wyze Cameras. You had better understand how to correctly interpret the timestamps and how your chosen tool might represent them. From a simplistic standpoint, FTK Imager does the best job of not possibly leading someone astray. An individual with a good understanding of FAT32 would have no issues using any of the tools as timestamps of 0000-00-00 00:00:00 would stand out and they would understand the reasons. PowerShell could be a little confusing as it gives a somewhat realistic looking timestamp even though FAT32 wasn't released until 1996 and FAT was released in 1977. Who knows, maybe you'll find yourself investigating a floppy disk from 1979!

| Tool | Result |
| --- | --- |
| FTK Imager | Doesn't display any accessed timestamp for FAT32. |
| Autopsy | Displays 0000-00-00 00:00:00 for Accessed timestamp. Timezone is set by investigator when ingesting the data source. |
| PowerShell | Displays 12/31/1979 11:00:00 PM for Accessed timestamp. |

