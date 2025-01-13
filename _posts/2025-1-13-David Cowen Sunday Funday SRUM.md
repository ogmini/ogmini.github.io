---
layout: post
title: David Cowen Sunday Funday Challenge - SRUM Validation 
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

David Cowen has started up his Sunday Funday challenges again and his latest one is related to SRUM. You can find his challenge on his [blog](https://www.hecfblog.com/2025/01/daily-blog-716-sunday-funday-11225.html).

## What is SRUM?

TO BE COMPLETED

## Test Setup

I'll be performing testing/validation using a Hyper-V VM running Windows 11 Pro 23H2 OS Build 22631.4602. The following tools will be used:

- [RawCopy](https://github.com/jschicht/RawCopy)
- [ESEDatabaseView](https://www.nirsoft.net/utils/ese_database_view.html)
- [SRUM Dump](https://github.com/MarkBaggett/srum-dump)

I also have three test files for the scenarios posed in the challenge. A simple text file, a 944 MB iso file, and a 5.41 GB iso file.

### Steps for each scenario
1. Delete the SRUM Database for a clean slate
2. Run scenario
3. Shutdown VM
4. Start VM
5. Run RawCopy and examine with ESEDatabaseView
6. Run SRUM Dump and examine output

The SRUM Database can be deleted by stopping the Diagnostic Policy Service (DPS), deleting the SRUDB.dat file, and starting the service back up. 

After running the scenario, the VM is shutdown properly in order to flush the data from the registry into the database file. 

## Scenario 1 - Copying data between two drives using copy and paste
In this scenario, I copy and paste the three files from the C: Drive to the E: Drive. 

INSERT VIDEO/GIF OF ACTION

![file timestamps](/images/scrum/file-timestamps.png)

Get-ChildItem | Select-Object Name, @{Name="LastWriteTime";Expression={$_.LastWriteTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="CreationTime";Expression={$_.CreationTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="LastAccessTime";Expression={$_.LastAccessTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}

As the screenshot shows, two of the files were created at 12:13:53 and the other files was created at 12:14:26 on the E: drive. This was a result of our copy and paste action.  

![file sizes](/images/scrum/file-sizes.png)

Get-ChildItem | Select-Object Name, Length, @{Name="LastWriteTime";Expression={$_.LastWriteTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="CreationTime";Expression={$_.CreationTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="LastAccessTime";Expression={$_.LastAccessTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}

The screenshot above shows the size in bytes of each of the three files and adding them up gives us 6,809,343,096 bytes. 

$files = Get-ChildItem
$totalSize = ($files | Measure-Object -Property Length -Sum).Sum
$totalSize

This information will be important to correlate back to the records found in the SRUM database. 

Examining the output of SRUM Dump, we can see a record showing 6,819,705,856 ForegroundBytesRead and 6,753,828,864 ForegroundBytesWritten for explorer.exe with a record creation time of 17:21:00 or 12:21:00. (Note: We must subtract 5 hours due to timezones as this testing is being done in the EST timezone).

![SRUM record](/images/scrum/srum_dump-1.png)

There are seemingly discrepancies in the ForegroundBytesRead, ForegroundBytesWritten, and actual size of the files. The times also do not appear to lineup at first glance. This is due to the 1 hour delay of records being flushed out of the registry and written to the SRUM database. I forced the flush to occur by shutting the system down around 12:21. In the screenshot below, the Event Log Service was stopped at 12:21:05.     

![shutdown time](/images/scrum/shutdown-time.png)

Just to verify the results from SRUM Dump, I examine the SRUM database using ESEDatabaseView as SRUM Dump does some processing to make the information more human readable. The SruDbIdMapTable shows the relation between AppId and the application location path. (Note: The IdBlob is stored as Hexadecimal. ESEDatabaseView is able to convert to text). In this case explorer.exe has an AppId of 192. 

![ese id map table](/images/scrum/ese-idmap.png)

Looking at the Application Resource Usage table, the same information that was extracted by SRUM Dump is visible in the SRUM database using ESEDatabaseView. 

![ese resource table](/images/scrum/ese-resource.png)

## Scenario 2 - Uploading data to an online service of your choice

## Scenario 3 - Wiping files