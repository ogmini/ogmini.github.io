---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence
author: 'ogmini'
tags:
 - Sunday-Funday
 - Challenge
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

Ended up not doing this

~~From my viewpoint, the following steps would be a little unrealistic for an attacker to actually perform. They would leave behind more artifacts though...~~

- ~~Open/verify the contents of the output file from LaZagne~~
- ~~Extract the file by uploading to an open NextCloud shared folder~~
- ~~Look for more artifacts~~

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

Windows Defender also has its own support logs located at `%ProgramData%\Microsoft\Windows Defender\Support\`. One thing that you might have noticed from the screenshots is a number **2147754837** that shows up in the Registry Keys, Event Logs, and the Support Logs. This ID Number can be used to follow/correlate records between the three sources. More importantly, this ID Number appears to be universal for the threat across systems. I had the same ID Number of **2147754837** on three different VMs for the same python file. Is this some internal ID Number on Microsoft's side or some other database? If anyone knows, I'm all ears.  

### LaZagne

![LaZagne](/images/browserpass/LaZagne.png)

Running LaZagne with the command line arguments `browsers -oN` kicks out a text file of results that contains the timestamp in the filename. The timestamp follows the format of DDMMYYY_HHMMSS. Sadly, nothing of note was found in the Shimcache or AmCache. We do get a hit on the Prefetch files using PECmd which is related to python being executed. The "Files Loaded" and "Directories" contain references to the path were LaZagne exists. Tenuous but something.

![Pefetch](/images/browserpass/prefetch.png)

### WebBrowserPassView

![WebBrowserPassView](/images/browserpass/WebBrowserPassView.png)

Windows Defender does not seem to block the execution of WebBrowserPassView. It does however log its execution with an EventID of 1160. This is not the case with LaZagne as we had added it as an Allowed Threat.  

![EventID 1160](/images/browserpass/event_passview.png)

Every time WebBrowserPassView is executed, it will create/update a WebBrowserPassView.cfg on closing the application. The MAC Timestamps for this file can prove useful as it potentially show first closing and last closing.  

![output](/images/browserpass/output_passview.png)

There are artifacts to be found in the Prefetch, Shimcache, Amcache, amd MUICache. All the standard caveats apply to these artifacts and much research has already been published on them. I'll provide some links in the References section below.

![Prefetch](/images/browserpass/prefetch_passview.png)

![Shimcache](/images/browserpass/appcompatcache_passview.png)

![Amcache](/images/browserpass/amcache_passview.png)

![MUICache](/images/browserpass/muicache.png)

### HackBrowserData

Windows Defender REALLY does not like this executable. Just downloading the release from the GitHub required adding two seperate Allowed Threat exceptions. Again, we see similar Event Logs to LaZagne of detecting the threat, quarantining the threat, and allowing the threat.

![Allowed Threats](/images/browserpass/hackbrowserdata_allowed.png)

![Registry](/images/browserpass/hackbrowserdata_registry.png)

![Event](/images/browserpass/hackbrowserdata_event.png)

Every time HackBrowserData is executed, it will create/update csv files in a results folder. The MAC Timestamps for this file can prove useful as it potentially shows first execution by looking at the folder and the last execution by looking at the csv files. In this test I just ran the executable from Windows Explorer the same way that WebBrowserPassView was tested.

![Screenshot](/images/browserpass/hackbrowserdata_screenshot.png)

There are artifacts to be found in the Prefetch and Amcache,  All the standard caveats apply to these artifacts and much research has already been published on them. I'll provide some links in the References section below.

![Prefetch](/images/browserpass/hackbrowserdata_prefetch.png)

![Amcache](/images/browserpass/hackbrowserdata_amcache.png)

## Conclusion

I didn't find any artifacts that would be considered out of the ordinary. Most of the artifacts centered around messing with Windows Defender to just get these tools to execute properly. Otherwise, they left some traces in a mixture of the Shimcache, Prefetch, Amcache, and MUICache.

| Tool | Windows Defender Artifacts | File Artifacts | Shimcache | Prefetch | Amcache | MUICache |
| --- | --- | --- | --- | --- | --- | --- |
| LaZagne (Python Version) | x | * | | x | | |
| WebBrowserPassView | x | * | x | x | x | x |  
| HackBrowserData | x | * | | x | x | |

Each of the tools did leave some sort of file system artifacts behind ranging from cfg files to folders. There is no guarantee these would be left behind and in the case of LaZagne it can be configured to not write to a file.

Sidenote, of the three tools I prefer LaZagne if I was just trying to get passwords from everything possible. HackBrowserData is a far more comprehensive tool for grabbing web browser data as it will get cookies, bookmarks, history, and much more. Windows Defender REALLY doesn't like it though.

## References

[Shimcache - 13Cubed](https://www.youtube.com/watch?v=7byz1dR_CLg)  
[Prefetch - 13Cubed](https://www.youtube.com/watch?v=f4RAtR_3zcs)
[Shimcache and Amcache - Magnet Forensics](https://www.magnetforensics.com/blog/shimcache-vs-amcache-key-windows-forensic-artifacts/)  
[MUICache -13Cubed](https://www.youtube.com/watch?v=ea2nvxN878s)
