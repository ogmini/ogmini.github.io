---
layout: post
title: GaslitPad - DNS Communication
author: 'ogmini'
tags:
 - malware 
 - GaslitPad
---

After releasing the first iteration of GaslitPad, I've been thinking about how to add some C2 or data transfer capability. The ideal case would be for the malware to send back information about opened files and more importantly, the unsaved buffer chunks as they are typed by the individual. You could essentially have a live window into the user's notepad session and see what they are typing as they type it. Always funny to me that MS essentially has a keylogger built into Windows Notepad. 

A few options would be to utilize:
- SSH to open a connection back to a server
- HTTPS to send request to a server
- Some bespoke P2P 
- DNS

I've been following Evan Anderson's articles on abusing DNS [https://www.offensivecontext.com/abusing-dns-part-1-how-does-dns-do-what-it-do/](https://www.offensivecontext.com/abusing-dns-part-1-how-does-dns-do-what-it-do/). This looks like a good option! Should be a good learning experience to implement this. 

## Previous Posts

[https://github.com/ogmini/Notepad-State-Library/releases/tag/GaslitPad](https://github.com/ogmini/Notepad-State-Library/releases/tag/GaslitPad)

[https://ogmini.github.io/2025/01/03/POC-Malware-Part-1.html](https://ogmini.github.io/2025/01/03/POC-Malware-Part-1.html)   
[https://ogmini.github.io/2025/01/31/POC-Malware-Part-2.html](https://ogmini.github.io/2025/01/31/POC-Malware-Part-2.html)  
[https://ogmini.github.io/2025/02/01/POC-Malware-Part-3.html](https://ogmini.github.io/2025/02/01/POC-Malware-Part-3.html)
[https://ogmini.github.io/2025/02/07/GaslitPad-Release.html](https://ogmini.github.io/2025/02/07/GaslitPad-Release.html)






