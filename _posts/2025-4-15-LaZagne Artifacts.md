---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence (LaZagne)
author: 'ogmini'
tags:
 - sunday funday
 - challenge
 - LaZagne
---

Messing around with Windows Defender just to download and execute LaZagne locally leaves artifacts behind related to exclusions. There are of course other more stealthy ways to run LaZagne by using RATs such as [Pupy](https://github.com/n1nj4sec/pupy/) or [Meterpreter/Metasploit](https://www.metasploit.com/). This post will list out the Registry Keys and Event Logs related to Windows Defender.

### Windows Defender 

For this testing, I downloaded the python script for LaZagne and Windows Defender alerted on and removed the files. I added an Exclusion Folder to Windows Defender. I did not use the standalone executable as it doesn't appear to have been compiled with all the modules required to grab browser passwords. Running the python script results in Windows Defender quarantining the file and I have to add it to the Allowed threats list. 

Exclusions can be found in the Registry at the following location `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Exclusions`

![Folder Exclusion](/images/browserpass/folder_exclusion.png)

Allowed Threats can be found in the Registry at the following location `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Defender\Threats`

![Allowed Threats](/images/browserpass/threat_allowed.png)

Event Logs related to Windows Defender are under Applications and Services Logs -> Microsoft -> Windows -> Windows Defender -> Operational. Of interest are:

| EventID | Notes |
| --- | --- |
| 5007 | Changing of the Registry Key with a new value |
| 1116 | Detection of Threat |
| 1117 | Quarantine of Threat |

![Event Exclusion](/images/browserpass/event_exclusion.png)

![Threat Detected](/images/browserpass/threat_detected.png)

![Threat Quarantined](/images/browserpass/threat_quarantined.png)

Windows Defender also has its own support logs located at `%ProgramData%\Microsoft\Windows Defender\Support\`. One thing that you might have noticed from the screenshots is a number **2147754837** that shows up in the Registry Keys, Event Logs, and the Support Logs. This ID Number can be used to follow/correlate records between the three sources. More importantly, this ID Number appears to be universal for the threat across systems. I had the same ID Number of **2147754837** on three different VMs for the same python file. Is this some internal ID Number on Microsoft's side or some other database? If anyone knows, I'm all ears.  

### LaZagne

![LaZagne](/images/browserpass/LaZagne.png)

Running LaZagne with the command line arguments `browsers -oN` kicks out a text file of results that contains the timestamp in the filename. The timestamp follows the format of DDMMYYY_HHMMSS. Sadly, nothing of note was found in the Shimcache or AmCache. We do get a hit on the Prefetch files using PECmd which is related to python being executed. The "Files Loaded" and "Directories" contain references to the path were LaZagne exists. Tenuous but something. 

![Pefetch](/images/browserpass/prefetch.png)