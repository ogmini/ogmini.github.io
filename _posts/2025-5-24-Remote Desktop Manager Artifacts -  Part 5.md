---
layout: post
title: Remote Desktop Manager - Artifacts Part 5
author: 'ogmini'
tags:
 - DFIR
 - Remote Desktop Manager
---

Stealing 15 minutes away from the family weekend. Remote Desktop Manager supports the use of a master key for security. It encrypts the data sources [https://docs.devolutions.net/rdm/commands/file/change-master-key/](https://docs.devolutions.net/rdm/commands/file/change-master-key/). What artifacts do we lose access to if the master key is set?

Looking at the `Connections` table, the DATA column is encrypted. The rest of the columns are NOT encrypted! Most importantly, the ID column is not encrypted. You can still relate this back to the `ConnectionLog` table which has no encrypted columns. The `ConnectionHistory` table only encrypts the DATA column but leaves the HistoryType and other columns unencrypted. 

Even with the master key set, you aren't completely left in the dark. There are still breadcrumbs and information to follow. 