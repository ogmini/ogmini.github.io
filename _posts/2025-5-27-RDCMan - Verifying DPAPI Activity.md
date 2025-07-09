---
layout: post
title: RDCMan - Verifying DPAPI Activity
author: 'ogmini'
tags:
 - DFIR
 - DPAPI
 - RDCMan
---

Followup to a previous [post](https://ogmini.github.io/2025/05/18/Audit-DPAPI-Activity.html) about logging DPAPI activity and verifying that RDCMan does leverage DPAPI/CryptProtectData to protect passwords. I'm happy to report that everything checks out and behaves as expected!

## Steps to Enable Relevant Logs

The earlier LinkedIn post glossed over how to enable the logs and the debug stream. The first step is to enable the Audit Events for both Success and Failure for the Audit DPAPI Activity. One way to accomplish this is by:

1. Open Local Group Policy Editor [https://www.thewindowsclub.com/how-to-open-group-policy-editor-in-windows-10](https://www.thewindowsclub.com/how-to-open-group-policy-editor-in-windows-10)
2. Navigate to Compuer Configuration -> Windows Settings -> Security Settings -> Advanced Audit Policy Configuration -> Detailed Tracking
3. Set "Audit DPAPI Activity" to both Success and Failure
4. Set "Audit Process Creation" to both Success and Failure

![gpedit2](/images/dpapi/gpedit2.png)

The second step is to enable the debug stream for Crypto-DPAPI. You could write a PowerShell script to do this; but I just typed the commands manually:

```PowerShell
$log = New-Object System.Diagnostics.Eventing.Reader.EventLogConfiguration Microsoft-Windows-Crypto-DPAPI/Debug
$log.IsEnabled = $True
$log.SaveChanges()
```

![debugenable](/images/dpapi/debugenable.png)

## Verification Steps

Once I made the above changes, I fired up RDCMan and added a new server with saved credentials using the default settings. This would result in RDCMan encrypting the password using DPAPI/CryptProtectData. I connected to the remote machine successfully and noted the time to make the logs easier to check. Below is what I found in Event View:

![16385](/images/dpapi/16385.png)

EventID 16385 located under Applications and Services Logs -> Microsoft -> Windows -> Crypto-DPAPI shows us the password being unprotected. There is an Operation Type of "SPCryptUnprotect" which also has a CallerProcessID of 300. This can be linked back to another event with an EventID of 4688 under Windows Logs -> Security

![4688](/images/dpapi/4688.png)

If we convert the CallerProcessID of 300 to hex we get 0x12c which matches the New Process ID of this event. This provides us the path to the executable that unprotected the password.  
