---
layout: post
title: Windows Notepad - Find/Replace/Bing
author: 'ogmini'
tags:
 - Research
 - Windows-Notepad
---

As is often the case, investigating something new leads to finding more new stuff that I hadn't noticed before. The `settings.dat` application hive stores a few more digital artifacts that I had not documented previously.  There are four more keys and a network/browser history artifact.

## Application Hive Keys

| KeyName | Type | Notes |
| --- | --- | --- |
| FindMatchCase | 0x5f5e10b | 0 = Off / 1 = On. Default is 0. |
| FindString | 0x5f5e10c | Stores the last string searched by find. |
| FindWrapAround | 0x5f5e10b | 0 = Off / 1 = On. Default is 1. |
| ReplaceString | 0x5f5e10c | Stores the last string that was the replacement. |

Find Registry Key  
![Find Registry](/images/windowsnotepad/find_registry.png)

Find Options Registry Keys
![Match Case Registry](/images/windowsnotepad/findmatchcase_registry.png)

![Word Wrap Registry](/images/windowsnotepad/findwrap_registry.png)

Replace Registry Key
![Replace Registry](/images/windowsnotepad/replace_registry.png)

A reminder that if a key doesn't exist in the application hive it means that the default has not been changed or touched. I would encourage testing this on your own so you can understand the nuance. For example, the FindString key won't exist if the user has never used the Find option. An important note about the Find and Replace registry keys is there is no direct correlation to the Replace string having replaced the Find string. They can get out of sync.

Find Dialog
![Find](/images/windowsnotepad/find.png)

Find Options
![Options](/images/windowsnotepad/findmatchwrap.png)

Replace Dialog
![Replace](/images/windowsnotepad/replace.png)

## Network / Browser History Artifact

Windows Notepad also has the option to "Search with Bing" where you highlight some text and it will obviously search Bing! All this options does is open your default browser to a Bing search URL following this format:

`https://www.bing.com/search?q=[SEARCHTERM]&form=NPNEW1`

The important thing to take note of is the query string being equal to `NPNEW1`. This is an indication that the search was performed from Windows Notepad. You can most likely also see this query string in any captured network traffic.  

![Bing](/images/windowsnotepad/bing.png)

## Behaviour

- FindString and ReplaceString keep the last string that was searched for and the replacement. If these keys don't exist, the options were never used.
- Using "Search with Bing" appends of a query string of `form=NPNEW1` to a Bing search.

## Importance

The Find registry key can identify what a user searched for last. Maybe a threat actor decided to LOTL and used Windows Notepad to examine a large configuration file. They searched for a specific phrase in order to find a password. This registry key would record that digital artifact. After finding a setting, the threat actor could use the Replace option to change all the instances of "<admin@company.com>" to "<evil@evil.com>". Evidence of this could be found in the Replace registry key.

The Bing artifact is a really evil one. Who uses Bing?! j/k
