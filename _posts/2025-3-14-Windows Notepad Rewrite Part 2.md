---
layout: post
title: Windows Notepad - Rewrite / AI Part 2
author: 'ogmini'
tags:
 - DFIR
 - windows-notepad
 - rewrite-AI
---

Continuing my research on Windows Notepad Rewrite [https://ogmini.github.io/2025/03/08/Windows-Notepad-Rewrite.html](https://ogmini.github.io/2025/03/08/Windows-Notepad-Rewrite.html). 

## New Application Hive Entry

| KeyName | Type | Notes |
|---|---|---|
|WebAccountId|0x5f5e10c| TBD

Haven't figured out what this value actually is referencing. It should be a string going by the type and the last 8 bytes are still the timestamp. In the screenshot below, I've obscured the value just in case it ends up being some private API key. It is 34 bytes long, it doesn't make sense for it to be a UUID format, and the strings don't match anything recognizable from my short research. Currently, I'm heading down the path of delving into Microsoft documentation:

- [https://learn.microsoft.com/en-us/uwp/api/windows.security.credentials.webaccount.id?view=winrt-26100](https://learn.microsoft.com/en-us/uwp/api/windows.security.credentials.webaccount.id?view=winrt-26100)

We'll see where that gets me.

![010 Editor view of new key](/images/rewrite/WebAccountID.png)   

## Network Traffic

It does not appear that Windows Notepad keeps any local record of any Rewrite calls to the Copilot API. Firing up Wireshark to record the network traffic during a Rewrite call nets us some potentially interesting packets.

1. DNS query for "apsaiservices.microsoft.com"
2. TLSv1.3 Handshakes
3. Encrypted data being sent to "apsaiservices.microsoft.com"
4. Encrypted data being sent back from "apsaiservices.microsoft.com"

![API Traffic](/images/rewrite/TLSv1.3Communication.png)   

At the moment, I'm not exactly sure how to decrypt this traffic. More learning to do:

- [https://wiki.wireshark.org/TLS#tls-decryption](https://wiki.wireshark.org/TLS#tls-decryption)

## Impact on TabState Files

There are no changes to the TabState files. The only giveaway that Rewrite was potentially used is if you were to examine the Unsaved Buffer Chunks while Windows Notepad was active. You would see a Deletion Action of all the characters of the original text, an Addition Action of all the new text provided by Rewrite, and a Cursor Position of 0. 

![UnsavedBufferChunks](/images/rewrite/ubc.png)  

## Open Question

Does Microsoft log these Rewrite calls? Can I access them?
