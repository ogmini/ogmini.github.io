---
layout: post
title: Remote Desktop Manager - Working on SQLECmd Map
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

In the middle of finishing up a SQLMap for [SQLECmd](https://github.com/EricZimmerman/SQLECmd). Handy tool that can pull out targetted information from mapped SQLite databases and export them to CSV/JSON for ingestion by other tools such as Timeline Explorer. Pretty straightforward process and if you can write a SQL query, you can make a SQLMap for SQLECmd.

I have general queries that pull connections, connection logs, attachments, and notes. I'm working on refining them down to just the information that an investigator would need in a time sensitive incident response situation or initial pass. More detailed analysis if/when warranted should be done by querying the database itself.   

As an example, looking at the connection logs would show the start and end timestamps for remote connections. There is even a handy column showing how long the session was active. Quick, triaged retrieval of this specific information could help greatly when responding to an incident. If a threat actor gained access to this system, did they use Remote Desktop Manager? What did they connect to? When did they connect? How long did they connect?

![ConnectionLog](/images/RemoteDesktopManager/connectionlog.png)