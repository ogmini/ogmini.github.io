---
layout: post
title: Researching RDCMan
author: 'ogmini'
tags:
 - DFIR
 - RDCMan
---

Going back many years, I've used [RDCMan](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman) or Remote Desktop Connection Manager extensively as a Systems Administrator. The price was right ($free) and it had all the features I needed to easily be able to remote into multiple servers. It is now a part of Sysinternals and up to version 3.1 as of this post. This looks like an interesting application to research from a forensic investigation standpoint. What artifacts does it leave behind and what can be gleaned from its configuration files. 

So far, I have found four artifacts:

### *.rdg files

Location: `Anywhere` (Default is My Documents)    
Format: XML

These files are XML files and store all the information related to remote desktop groups in RDCMan. I will provide more details in a later post. An encrypted password can be stored in these files. There are two options for encryption using [CryptProtectData](https://learn.microsoft.com/en-us/windows/win32/api/dpapi/nf-dpapi-cryptprotectdata) or an x509 Certificate. CryptProtectData is the default setting. 

### Version Folders

Location: `%localappdata%\Microsoft\Remote Desktop Connection Manager`    
Format: Folder Names

From initial glance, it appears that RDCMan will create seemingly empty folders tied to the version of RDCMan being run. 

![RDCMan Version Folders](/images/RDCMan/rdcmanversions.png)

### RDCMan.settings file

Location: `%localappdata%\Microsoft\Remote Desktop Connection Manager`    
Format: XML

This is another XML file that stores global settings for RDCMan. This includes the encryption option for how passwords are stored. Knowing which encryption is being used gives the option to identify the certificate in use and possibly export it.

![RDCMan Settings](/images/RDCMan/rdcmansettings_cert.png)

### Personal Certificate 

Location: `%appdata%\Microsoft\SystemCertificates\My\Certificates`

The RDCMan documentation does a much better job at explaining this file and its importance. [https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman#encryption-settings](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman#encryption-settings). If you have the ability, exporting the certificate would be a good idea. Something to explore is the possiblity of retrieving the cleartext password. 

![Personal Certificate](/images/RDCMan/personalcert.png)

## References 
- [https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman)
