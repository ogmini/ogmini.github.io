---
layout: post
title: GaslitPad - Release
author: 'ogmini'
tags:
 - malware 
 - GaslitPad
---

![GaslitPad](/images/malware/Gaslitpad-logo.jpg)

First release of the Proof of Concept Malware called GaslitPad for Windows Notepad. 

[https://github.com/ogmini/Notepad-State-Library/releases/tag/GaslitPad](https://github.com/ogmini/Notepad-State-Library/releases/tag/GaslitPad)

[https://ogmini.github.io/2025/01/03/POC-Malware-Part-1.html](https://ogmini.github.io/2025/01/03/POC-Malware-Part-1.html)   
[https://ogmini.github.io/2025/01/31/POC-Malware-Part-2.html](https://ogmini.github.io/2025/01/31/POC-Malware-Part-2.html)  
[https://ogmini.github.io/2025/02/01/POC-Malware-Part-3.html](https://ogmini.github.io/2025/02/01/POC-Malware-Part-3.html)

## Options

These can be set by editing the "GastlitPad.dll.config" file. The default settings will perform an Active Attack on a file called "wp-config.php" after 10 seconds of idle time.

- attackVersion - 0 is Active Attack / 1 is Sleep Attack 
- attackFileName - Filename to attack
- attackRegex  - regex for text to replace 
- attackReplace - text to replace regex match
- idleWaitTime - idle wait time in seconds for Active Attack
- pollingInterval - polling time in milliseconds to check 

## Attack Demonstrations

An example "wp-config.php" file has been included in the zip file to demonstrate the attack in action.

### Active Attack

Make sure the options are set for the Active Attack. Run GaslitPad how you see fit. Open "wp-config.php" in Windows Notepad and make a change to the file. Do not save the file or close Windows Notepad. Wait the required idleWaitTime without any actions and you should see Windows Notepad blink and the text change to 'compromised'. 

### Sleep Attack

Make sure the options are set for the Sleep Attack. Open "wp-config.php" in Windows Notepad and make a change to the file. Do not save the file and instead just close Windows Notepad. Wait a second or two and the Tab State will be changed. You can reopen Windows Notepad and see that the text has been changed to 'compromised'




