---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence (HackBrowserData)
author: 'ogmini'
tags:
 - sunday funday
 - challenge
 - HackBrowserData
---

Today, we look at [HackBrowserData](https://github.com/moonD4rk/HackBrowserData) as part of the [Sunday Funday Challenge](https://ogmini.github.io/2025/04/14/David-Cowen-Sunday-Funday-Browser-Password-Extraction.html). Nothing groundbreaking but a good exercise in double checking and verifying understanding and artifacts. 

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

## References

[Shimcache - 13Cubed](https://www.youtube.com/watch?v=7byz1dR_CLg)  
[Prefetch - 13Cubed](https://www.youtube.com/watch?v=f4RAtR_3zcs)   
[Shimcache and Amcache - Magnet Forensics](https://www.magnetforensics.com/blog/shimcache-vs-amcache-key-windows-forensic-artifacts/)  
[MUICache -13Cubed](https://www.youtube.com/watch?v=ea2nvxN878s)