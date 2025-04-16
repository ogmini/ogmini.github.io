---
layout: post
title: David Cowen Sunday Funday Challenge - Browser Password Extraction Evidence (WebBrowserPassView)
author: 'ogmini'
tags:
 - sunday funday
 - challenge
 - WebBrowserPassView
---

Today, we look at WebBrowserPassView from NirSoft as part of the [Sunday Funday Challenge](https://ogmini.github.io/2025/04/14/David-Cowen-Sunday-Funday-Browser-Password-Extraction.html). Nothing groundbreaking but a good exercise in double checking and verifying understanding and artifacts. 

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

## References

[Shimcache - 13Cubed](https://www.youtube.com/watch?v=7byz1dR_CLg)  
[Prefetch - 13Cubed](https://www.youtube.com/watch?v=f4RAtR_3zcs)   
[Shimcache and Amcache - Magnet Forensics](https://www.magnetforensics.com/blog/shimcache-vs-amcache-key-windows-forensic-artifacts/)  
[MUICache -13Cubed](https://www.youtube.com/watch?v=ea2nvxN878s)