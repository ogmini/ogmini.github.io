---
layout: post
title: Remote Desktop Manager - Artifacts Part 3
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

Busy day at work today. Didn't have much bandwidth to poke around Remote Desktop Manager. I've always found it useful to utilize Database Diagrams to both understand and explain a database's data structure. For SQLite databases, I use a tool called [Dbeaver](https://dbeaver.io/) to easily auto-generate the Database Diagram for me and give me a good base to start better documentation. Oddly, I still prefer using SQLite Browser to examine the data. I've attached the generated image below.

![Database Diagram](/images/RemoteDesktopManager/dbdiagram.png)

Now remember, this Database Diagram is auto-generated and SQLite does not enforce relationships between tables in the same way as more robust RDBMS systems like PostgreSQL or MySQL. This requires manually fixing. Just as an example, Attachment records have a "ConnectionID" which references the "ID" in the Connections records. This relationship is not represented in the Database Diagram. Dbeaver is taking guesses the best it can.
