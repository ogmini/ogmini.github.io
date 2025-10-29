---
layout: post
title: BelkaCTF 7 - AAR Radars
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task18.png)

For this task, we need to find which ATC Radars detected a UFO and so we are going back to our memory image of the ATC computer. I will not be rewriting everything as this builds upon the same tools/commands/steps used in previous writeups that you can find at [https://ogmini.github.io/tags.html#Belkasoft](https://ogmini.github.io/tags.html#Belkasoft). Using MemProcFS, I mount the memory dump and run it in forensic mode.

``` cmd
MemProcFS.exe -device C:\BelkaCTF7\BelkaCTF_7_CASE250722_KTSOAERO.mem -forensic 1
```

Looking at the running processes, I see a few called ATC_RADAR.exe and this seems like a good place to start as I would assume this is the ATC software running on the ATC computer.

![ATC_RADAR.exe](/images/BelkaCTF7/Task18-1.png)

I pick one of them and examine the win-*.txt files to gather more details about the process including the commandline that launched the process and the path to the executable. All of this can be useful as programs often store information alongisde the executable and it gives us a place to start looking for relevant files.

![win-txt](/images/BelkaCTF7/Task18-2.png)

Popping open the `win-cmdline.txt` file gives us a lot of information. And I focus in on one of the commandline arguments of --app-path="C:\Users\award\AppData\Local\Program\ATC_RADAR\resources\app".

![folder](/images/BelkaCTF7/Task18-3.png)

![files](/images/BelkaCTF7/Task18-4.png)

Going to that location reveals a `ffffe6892bd4a3f0-radar_data.db` file and I open it in DB Browser for SQLite and start looking at tables and data. There is an `object` table which appears to list contacts along with a name and ID. Helpfully, there is a contact called "UFO137" which has an ID of 36.

![objects](/images/BelkaCTF7/Task18-5.png)

There are also two other tables called `detects` and `radar_serials` that can all be joined to create the following query which gives you the Serial Numbers of the ATC Radars that detected the UFO:

```sql
SELECT serial_number FROM detects 
INNER JOIN radar_serials
ON detects.radar_id = radar_serials.radar_id
WHERE aircraft_id = 36
```

RDAD425319469B
RDA665200514A
RNN770196044B
RDA714203872B

This can also be done using Volatility 3 and the filescan and dumpfiles plugins.

```cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.filescan
```

Using the filescan plugin, we can find the virtual address of the radar_data.db file that we are interested in and use the dumpfiles plugin to dump the file.

```cmd
vol -f BelkaCTF_7_CASE250722_KTSOAERO.mem windows.dumpfiles --virtaddr 253476792055168
```

The rest of the steps follow the above.

## Thoughts

Another fun one that again highlights the pros/cons of using different tools.
