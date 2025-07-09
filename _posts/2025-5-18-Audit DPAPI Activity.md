---
layout: post
title: DPAPI - Audit DPAPI Activity
author: 'ogmini'
tags:
 - DFIR
 - DPAPI
 - RDCMan
---

Came across a [post](https://www.linkedin.com/feed/update/urn:li:activity:7328498240952168450/) on LinkedIn recently about enable "Audit DPAPI Activity" in order to see event logs related to DPAPI calls. The post is coming from the standpoint of detecting anomalous actions in a SIEM or other tool.

This is also useful to while investigating applications and what capabilities they might leverage. I'm specifically thinking about RDCMan, which we already know by their documentation that it leverages DPAPI/CryptProtectData to protect passwords. If you didn't know, this would be one easy way to tell if the process calls DPAPI.

When I get a chance, I'll test this out and see if RDCMan leaves any event logs related to DPAPI activity.

Just in case the post is ever lost/deleted.

> DPAPI is Microsoft's builtin symmetric-crypto wrapper-the mechanism that for example browsers use for saved credentials and Windows uses for stored Wi-Fi keys-but attackers lean on the same API to unwrap credentials after they land. If you enable "Audit DPAPI Activity" under Security Settings > Advanced Audit Policy Configuration > Detailed Tracking you get Event ID 4693 whenever a secret is unprotected, yet that log entry only shows lsass.exe because LSASS brokers the call.
>
> Switch on the Crypto-DPAPI debug stream and you gain Event 16385, which includes the calling process ID in decimal. Chain that PID to the matching Process Creation record (Event 4688) and you can pivot straight to the executable that made the DPAPI call. With both channels forwarded to your SIEM it is trivial to build a rule: if any user-mode process other than the usual suspects invokes unprotect, raise an alert.

## References

- [https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/audit-dpapi-activity](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-10/security/threat-protection/auditing/audit-dpapi-activity)
- [https://www.linkedin.com/feed/update/urn:li:activity:7328498240952168450/](https://www.linkedin.com/feed/update/urn:li:activity:7328498240952168450/)
- [https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/Default.aspx?catid=3&subcatid=33](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/Default.aspx?catid=3&subcatid=33)
