---
layout: post
title: David Cowen Sunday Funday Challenge - SRUM Validation 
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

David Cowen has started up his Sunday Funday challenges again and his latest one is related to SRUM. You can find his challenge on his [blog](https://www.hecfblog.com/2025/01/daily-blog-716-sunday-funday-11225.html). This post is in progress and will be updated.

## What is SRUM?

TO BE COMPLETED

## Test Setup

I am performing testing/validation using a Hyper-V VM running Windows 11 Pro 23H2 OS Build 22631.4602. The following tools are being used:

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

![file timestamps](/images/srum/file-timestamps.png)

~~~ Powershell
Get-ChildItem | Select-Object Name, @{Name="LastWriteTime";Expression={$_.LastWriteTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="CreationTime";Expression={$_.CreationTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="LastAccessTime";Expression={$_.LastAccessTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}
~~~

As the screenshot shows, two of the files were created at 12:13:53 and the other files was created at 12:14:26 on the E: drive. This was a result of our copy and paste action.  

![file sizes](/images/srum/file-sizes.png)

~~~ Powershell
Get-ChildItem | Select-Object Name, Length, @{Name="LastWriteTime";Expression={$_.LastWriteTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="CreationTime";Expression={$_.CreationTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}, @{Name="LastAccessTime";Expression={$_.LastAccessTime.ToString("yyyy-MM-dd HH:mm:ss.fff")}}
~~~

The screenshot above shows the size in bytes of each of the three files and adding them up gives us 6,809,343,096 bytes. 

~~~ Powershell
$files = Get-ChildItem
$totalSize = ($files | Measure-Object -Property Length -Sum).Sum
$totalSize
~~~

This information will be important to correlate back to the records found in the SRUM database. 

Examining the output of SRUM Dump, we can see a record showing 6,819,705,856 ForegroundBytesRead and 6,753,828,864 ForegroundBytesWritten for explorer.exe with a record creation time of 17:21:00 or 12:21:00. (Note: We must subtract 5 hours due to timezones as this testing is being done in the EST timezone).

![SRUM record](/images/srum/srum_dump-1.png)

There are seemingly discrepancies in the ForegroundBytesRead, ForegroundBytesWritten, and actual size of the files. The times also do not appear to lineup at first glance. This is due to the 1 hour delay of records being flushed out of the registry and written to the SRUM database. I forced the flush to occur by shutting the system down around 12:21. In the screenshot below, the Event Log Service was stopped at 12:21:05.     

![shutdown time](/images/srum/shutdown-time.png)

Just to verify the results from SRUM Dump, I examine the SRUM database using ESEDatabaseView as SRUM Dump does some processing to make the information more human readable. The SruDbIdMapTable shows the relation between AppId and the application location path. (Note: The IdBlob is stored as Hexadecimal. ESEDatabaseView is able to convert to text). In this case explorer.exe has an AppId of 192. 

![ese id map table](/images/srum/ese-idmap.png)

Looking at the Application Resource Usage table, the same information that was extracted by SRUM Dump is visible in the SRUM database using ESEDatabaseView. 

![ese resource table](/images/srum/ese-resource.png)

The GUID for the table name references registry keys that give the human readable representation.

![registry](/images/srum/registry-application.png)

### Open Questions

The three files on disk are only 6,809,343,096 bytes. 55,514,232 less bytes were written and 10,362,760 more bytes were read. The record in the SRUM database is specifically for the application explorer.exe and not recorded at a lower level that would show us specific actions. It is aggregating multiple actions involving explorer.exe into one record. This must always be kept in mind when looking at this data as it is only really recording disk utilization in terms of all reads/writes over the past hour or prior to shutdown. 

The time discrepancy makes sense. I do wonder if there is a way to force a flush without shutting down the computer or can you simply change the system time.

## Scenario 2 - Uploading data to an online service of your choice

For this scenarion, I uploaded the 944 MB iso file to https://www.file.io/ at 2:14 PM. I am not endorsing this website or its usage. It was just convenient for testing purposes. 

INSERT VIDEO OF ACTION

Examining the output of SRUM Dump, we can see a record showing 1,003,993,221 Bytes Sent for msedge.exe with a record creation time of 19:23:00 or 14:23:00. (Note: We must subtract 5 hours due to timezones as this testing is being done in the EST timezone). 

![SRUM record](/images/srum/srum_dump-2.png)

Once again, we have similar discrepancies to scenario 1 for exactly the same reasons. If we examine the creation times in the SRUM database we can see that approximately hourly pattern appearing with times of:
- 17:20
- 18:21
- 19:23

![SRUM flushes](/images/srum/srum_flushes.png)

Again, I verify the results using ESEDatabaseView. In this case msedge.exe has an AppId of 177. 

![ese id map table](/images/srum/ese-idmap2.png)

Looking at the Network Data Usage table, the same information that was extracted by SRUM Dump is visible in the SRUM database using ESEDatabaseView. We can also see me downloading some other files in a prior 1 hour window.  

![ese resource table](/images/srum/ese-resource2.png)

The GUID for the table name references registry keys that give the human readable representation.

![registry](/images/srum/registry-network.png)

### Open Questions

The file that was uploaded had a file size of 989,855,744 bytes. 14,137,477 more bytes were sent by Microsoft Edge. Again, the record in the SRUM database is specifically for the application msedge.exe and not recorded at a lower level that would show us specific actions. It is aggregating all the network traffic involving msedge.exe into one record. This must alawys be kept in mind when looking at this data as it is only really recording network usage in terms of data sent and recieved over the part hour or prior to shutdown.  

The time discrepancy makes sense. I do wonder if there is a way to force a flush without shutting down the computer or can you simply change the system time.

## Scenario 3 - Wiping files

For this scenario, I "permanently" deleted the 944 MB iso file at 2:57 PM. 

INSERT VIDEO OF ACTION

Examining the output of SRUM Dump, we can see a record showing 1 ForegroundNumberOfFlushes for explorer.exe with a record creation time of 20:23:00 or 15:23:00. (Note: We must subtract 5 hours due to timezones as this testing is being done in the EST timezone). 

![SRUM record](/images/srum/srum_dump-3.png)

Again, I verify the results using ESEDatabaseView. The AppId is still 192. Looking at the Application Resource Usage table, the same information that was extracted by SRUM Dump is visible in the SRUM database using ESEDatabaseView. 

![ese resource table](/images/srum/ese-resource3.png)

The GUID for the table name references registry keys that give the human readable representation.

![registry](/images/srum/registry-application.png)

### Open Questions

The SRUM database doesn't store the amount of bytes deleted and this makes sense. The action of deleting a file would only involve marking the file as deleted. The Operating System will not be writing or reading the full file to accomplish this aciton. 

## Conclusion

For the records examined in the SRUM database, the data is skewed due to the actual usage by the Operating System. Timestamps will be tied to an hourly schedule or shutdown times. Bytes read and written by applications are aggregated over the hour window while deletions are recorded in a counter. 

I would like to validate the following applications given time:
- [SrumECmd](https://github.com/EricZimmerman/Srum)
- [Velociraptor](https://docs.velociraptor.app/)

I'm also interested to know where the data exists before it is flushed to the SRUM database and if it is possible to read it prior to the flush. 
