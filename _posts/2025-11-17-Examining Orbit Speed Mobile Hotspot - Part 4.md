---
layout: post
title: Examining Mobile Hotspots - Orbic Speed RC400L - Part 4
author: 'ogmini'
tags:
 - Mobile-Hotspot
 - ADB-Scripts
 - RC400L
---

Came across an interesting SQLite database called `cpe.db` on the mobile hotsport as I was starting to examine all the files under the `/usrdata/data/usr` folder. It contained old SMS messages! In my case, these were spam SMS messages that I had ignored and never bothered to read/delete when I was still using this hotspot actively.

The database contains the following tables:

| Table | Fields | Notes |
| --- | --- | --- |
| DBTABLECASCADE | ID, PHONE, REFNUM, CASNUM, TIMESTAMP, CONTENT | Empty |
| DBTABLEPHONEBOOK | ID, NAME, MOBILENUMBER, HOMENUMBER, OFFICENUMBER, EMAIL, XGROUP | Empty |
| DBTABLEPHONEBOOKGROUP | ID, NAME | Empty |
| DBTABLESMS | ID, PHONE, DIRECTION, STATUS, TIME, CONTENT | SMS Messages. Time is in Unix format. Direction 1 = Recieved. Status 0 = Unread |

This is a rather intriguing digital artifact. If a suspect has an old hotspot that hasn't been connected to the cellular network can we recover potentially deleted SMS messages or phone contacts? I need to see if I can setup contacts and get those other tables populated. This is just another example of why turning on a mobile device should be done with care. If the SIM card is present or it is able to make a network connection, what digital artifacts could be altered?

You can find all the previous posts at [https://ogmini.github.io/tags.html#Mobile-Hotspot](https://ogmini.github.io/tags.html#Mobile-Hotspot).
