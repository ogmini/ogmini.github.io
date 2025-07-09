---
layout: post
title: SSD Forensics - Flex Capacity
author: 'ogmini'
tags:
 - Musings
---

I saw a recent [post](https://blog.elcomsoft.com/2025/06/what-trim-drat-and-dzat-really-mean-for-ssd-forensics/) by Oleg Afonin about the implications of how TRIM works on various SSDs. I would suggest reading his article as it dives into some very important distinctions when potentially imaging SSDs. It reminded me of a research article I came across during my classes at Champlain.

The research [article](https://arxiv.org/pdf/2112.13923) was examining the Flex Capacity capability of certain Micron SSDs and its potential for misuse in malware or other attacks. The article really caught my imagination and for a bit I harbored the illusion of doing my Master's capstone on seeing how data could be recovered from the over provisioning space. I went so far as to buy 2 of the drives. I did a little bit of research and quickly realized it would not be very straightforward. Any research would require some pretty specialized software, proprietary knowledge, and LOTS of testing. None of this would fit within the timeframe of the capstone course.

During my research, I did come across [http://ata-atapi.com/](http://ata-atapi.com/) which is a tool for testing that allows you to send direct commands via ATA. This wouldn't solve the problem of needing to know vendor/firmware specific commands. They've also released a newer version at [https://www.hiperfstore.com/](https://www.hiperfstore.com/) that works on Linux. I've no idea the cost and I'm afraid to even ask them for a quote as a pure hobbyist.

This is something that still interests me that is just held back by my current knowledge. I've always wondered if a nation state actor could attack the supply chain for SSDs by applying a compromised firmware in order to hide malware.
