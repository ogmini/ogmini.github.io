---
layout: post
title: Researching RDCMan - Part 2
author: 'ogmini'
tags:
 - DFIR
 - RDCMan
---

Andrew Rathbun pointed out an interesting anomaly that I didn't pick up on related to the versioning. Sysinternals announced on their [blog](https://techcommunity.microsoft.com/blog/sysinternals-blog/rdcman-v3-0-and-sysmon-1-3-6-for-linux/4410914) on 5/5/2025 that RDCMan v3.0 was released. The version listed on the [documentation](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman) which was updated on 5/5/2025 was v3.1. Maybe just a quick stealth patch. Checking the executable shows the version as 3.1.0.0 and it reports as v3.1 from the About.

![RDCMan Version 3.1](/images/RDCMan/RDCMan_v3.1.png)

Again, I'm going to complain about the ability to get previous versions for testing and the lack of a centralized changelog. You can already see some of that confusion with v3.0 on the announcement and v3.1 on the documentation. The documentation also makes note of incompatiblity with older *.rdg files. I'm unable to test this at the moment as I don't have any version that appears to be old enough for this to happen. There is potentially a very niche digital artifact in the form of*.rdg.old files which are created when the files are updated to the new version.

Below is the start of detailing some of forensic relevant information and behaviour found within the *.rdg files. It is important to note that there are three levels of settings in order of hierarchy:

1. Global
2. Group
3. Server

![Hierarchy](/images/RDCMan/hierarchy.png)

The Global settings will be stored in the RDCMan.settings file and the Group/Server settings will be stored in the associated *.rdg file.

### *.rdg Details

Currently, the XML file has a programVersion of 3.1 and a schemaVersion of 3.

``` XML
<RDCMan programVersion="3.1" schemaVersion="3">
```

An *.rdg file consists of various elements such as:

- properties - Includes information such as name and comment
- logonCredentials - Includes information such as username, encrypted password, domain
- server - Can be multiple elements. Includes information such as address of server and comment
- connected - Listing of connected sessions
- favorites - Listing of favorite servers

NOTE: Remember that settings can be inherited! For example, the logon credentials could be at a group or even global level. If that is the case, you will not see those elements at the server level.

The connected element is interesting in that it lists any servers that still had a connection when RDCMan was closed and the *.rdg file was saved.

### RDCMan.settings Details

Only had a little bit of time to look deeper into this file. Much like the *.rdg file, there is a programVersion of 3.1

```XML
<Settings programVersion="3.1">
```

There looks to be some interesting elements in this file such as:

- FilesToOpen - Lists the locations of opened/active RDG groups
- encryptionSettings - Specifies LogonCredentials for CryptProtectData or Certificate for the x509 Certificate

### Version Folders

I made a correction to yesterday's post regarding the Version Folders. I've included that section below for ease.

Location: `%localappdata%\Microsoft\Remote Desktop Connection Manager`
Format: Folder Names

From initial glance, it appears that RDCMan will create seemingly empty folders tied to the version of RDCMan being run.  

CORRECTION - The folder that is created is for the version of RDCMan that was launched without a RDCMan.settings file previously existing. This is an interesting distinction.

## References

- [https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman](https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman)
