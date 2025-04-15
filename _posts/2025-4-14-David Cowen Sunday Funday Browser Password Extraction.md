---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

Another week, another David Cowen Sunday Funday challenge posted at his [blog](https://www.hecfblog.com/2025/04/daily-blog-807-sunday-funday-41325.html) and it is about looking for artifacts left behind by browser password extraction programs such as WebBrowserPassView. 

Can I defend my streak? I'm not in this to win anything. These challenges provide a good way to focus on different areas and work on methodology. I'm really hoping for others to participate as I feel I can learn a lot more from their writeups. 

## Challenge

It's becoming more common that the first thing an attacker will try to do if they get access to a user's system is extract all of the saved browser passwords. Profile a popular browser password extractor (such as WebBroweerPassView or HackBrowserData) and detail what artifacts are left behind that would reveal their usage on a Windows 11 system. Extra points if you:
a. Try multiple browser password viewing tools
b. Try MacOS as well as Windows

## Test Setup

The browser password extractor is being left up to us though we were given two examples. I will be testing for artifacts left behind by [LaZagne](https://github.com/AlessandroZ/LaZagne). I will not be doing anything fancy to obfuscate or hide the execution of the program. As always, Windows 11 24H2 will have a standard installation. If I get feisty, we'll delve into other ones such as the aforementioned [WebBrowserPassView](https://www.nirsoft.net/utils/web_browser_password.html).

1. Save some passwords into the Edge Browser
    - At this point, most of the popular browser are all Chromium based.
2. Execute `LaZagne.exe browsers -oN`
    - We may need to put Windows Defender exclusions into place.
3. Look for artifacts

### Unrealistic Steps

From my viewpoint, the following steps would be a little unrealistic for an attacker to actually perform. They would leave behind more artifacts though...

4. Open/verify the contents of the output file from LaZagne
5. Extract the file by uploading to an open NextCloud shared folder
6. Look for more artifacts

## Assumptions

Honestly, I'll be surprised if we can find any weird artifacts. I may have to add Windows Defender exclusions to let the program run. An attacker could customize and compile their own version but that will be out of scope of this testing. This could make it harder to detect. I will look for artifacts of these exclusions being put into place and later removed. 

There might be some artifacts left in the Prefetch files, Shimcache, and Amcache. I don't expect to see much in the Event Logs; but it is worth checking. 

## Testing

Messing around with Windows Defender just to download and execute LaZagne locally leaves artifacts behind related to exclusions. There are of course other more stealthy ways to run LaZagne by using RATs such as [Pupy](https://github.com/n1nj4sec/pupy/) or [Meterpreter/Metasploit](https://www.metasploit.com/). 

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

### LaZagne

![LaZagne](/images/browserpass/LaZagne.png)

Running LaZagne with the command line arguments `browsers -oN` kicks out a text file of results that contains the timestamp in the filename. The timestamp follows the format of DDMMYYY_HHMMSS. Sadly, nothing of note was found in the Shimcache or AmCache. We do get a hit on the Prefetch files using PECmd which is related to python being executed. The "Files Loaded" and "Directories" contain references to the path were LaZagne exists. 