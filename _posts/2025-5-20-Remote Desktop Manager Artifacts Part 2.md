---
layout: post
title: Remote Desktop Manager - Artifacts Part 2
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

I submitted a Kape Target for Remote Desktop Manager. I fully expect that I'll be making changes to the Target even if it is to just update the documentation. Since yesterday's post, I've found a few things that I missed/glossed over initially. 

## Data Sources

Remote Desktop Manager can read/store connection information from different types of Data Sources. I'm only testing the free edition at the moment so only have access to a subset of the potential Data Sources. There are both Individual and Shared Data Sources and I've only looked at the Individual ones so far.

- Individual
    - SQLite
    - XML
    - Devolutions Hub Personal (Cloud)
- Shared
    - CyberArk (preview)
    - Devolutions Hub Business
    - Devolutions Server
    - Microsoft Azure SQL
    - Microsoft SQL Server

In the first [post](https://ogmini.github.io/2025/05/19/Remote-Desktop-Manager-Artifacts.html), we only talked about the SQLite Date Source which by default is called "Connections.db". A user can create more SQLite Data Sources and choose to store them at any path/filename of their choosing. By default, it will save to `%localappdata%\Devolutions\RemoteDesktopManager\`. 

The XML Data Source is also stored in the same location with a default name of "Connections.xml". Again, a user can create more XML Data Sources and choose to store them at any path/filename of their choosing. 

Interestingly, the `Connections.log` log appears to be a unified log across any/all Data Sources. 