---
layout: post
title: BelkaCTF 7 - AAR Workspace
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task9.png)

We need to find out where the owner of the phone works and their position. From the previous task, we have some areas to examine that might find us our answer:

- Email boxes related to `googleslover93@gmail.com` and `stonepresspa@runbox.com`
- WhatsApp
- Telegram

![Accounts](/images/BelkaCTF7/Task8-1.png)

Always worth browsing any communications on a device as a very early step. Unfortunately, ALEAPP doesn't appear to be able to retrieve the digital artifacts for the `stonepresspa@runbox.com` mailbox so we must go digging manually.

Navigating to `\data\com.google.android.gm\databases` we come find an `EmailProviders.db` file which we can open up in DB Browser for SQLite. Looking at the Message table, we find an email sent from `stonepresspa@runbox.com` to a `d.ragowski@propublica.org` and in the body it contains the name of the workplace and their position.

Investgative Journalist  
Peach State Ledger

![Email](/images/BelkaCTF7/Task9-1.png)

Using BelkaSoft was much easier as it finds the IMAP mailbox and parses out the Sent emails from the database.

![BelkaSoft Email](/images/BelkaCTF7/Task9-2.png)

## Thoughts

Always good to verify. I'm actually surprised that ALEAPP didn't parse out this artifact. Maybe something to contribute as a plugin.
