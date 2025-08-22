---
layout: post
title: BelkaCTF 7 - AAR Junk Messaging
author: 'ogmini'
tags:
 - CTF
 - Belkasoft
 - Writeups
---

![Task](/images/BelkaCTF7/Task11.png)

In this task, we have to search the chat logs for the mention of some sort of addictive substance. This ended up not being as simple as it sounds. Again, I used ALEAPP and saw that the suspect was using WhatsApp to communicate with other individuals. Browsing through the chat logs did surface some very odd messages that were mixes of languages... (Starting to get some Stranger Things vibes). This was not going to be easy unless you can read all those different languages.

![Strange Messages](/images/BelkaCTF7/Task11-1.png)

At first, I tried just browsing the messages for anything that leaped out and quickly abandoned that for trying to search for words like "drug" and "dependency". I got lucky and got a hit in which the suspect used the word "dependency" in the english language which references C8H10N4O2 which is caffeine - [https://webbook.nist.gov/cgi/cbook.cgi?ID=C58082&Mask=4](https://webbook.nist.gov/cgi/cbook.cgi?ID=C58082&Mask=4).

![Caffeine](/images/BelkaCTF7/Task11-2.png)

## Thoughts

Part of me feels like I got real lucky with my search and that this task was created to really hammer home the potential usefulness of "AI" in investigations. The AAR for the next task will also highlight this point as I was unable to solve it during the timeframe and "AI" would definitely have helped. At one point, I was copy/pasting the messages into ChatGPT to translate the messages which of course is incredibly tedious.
