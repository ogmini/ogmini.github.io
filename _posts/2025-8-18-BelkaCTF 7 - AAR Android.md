---
layout: post
title: BelkaCTF 7 - AAR Android
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task8.png)

A new piece of evidence in the form of an extract from an Android device. Starting with the simple task of finding out the Google account of the owner. I load it up in ALEAPP and check under the "Accounts ce" section where we find a com.google account for `goggleslover93@gmail.com`.

![ALEAPP](/images/BelkaCTF7/Task8-1.png)

The nice thing about ALEAPP is that it is very upfront about where it found the digital artifact. In this case, it parsed the information out of the `accounts_ce.db` SQLite database.

## Thoughts

Pretty straightforward task. If you're interested in more details about the various digital artifacts related to user accounts on Android there is a good article on Belkasoft's website [https://belkasoft.com/android-system-artifacts-forensic-analysis-of-application-usage#accounts](https://belkasoft.com/android-system-artifacts-forensic-analysis-of-application-usage#accounts).

One note to remember is the AFU vs BFU state of a phone as it could have implications on the recovery of digital artifacts. The `accounts_ce.db` file is part of the Credential Encrypted storage on Android and therefore it is only decrypted/accessible After First Unlock (AFU). In this case, the phone was unlocked at some point after being rebooted. This is also why Graphene OS has an [auto-reboot feature](https://grapheneos.org/features#auto-reboot) as they want to lessen the time a phone is in the AFU state.
