---
layout: post
title: Remote Desktop Manager - Artifacts
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

Remote Desktop Manager from Devolutions is an alternative to RDCMan that offers more features and centralized capabilities. You can read more about the them on their website - [https://devolutions.net/remote-desktop-manager/](https://devolutions.net/remote-desktop-manager/). Today, I'll be looking at version 2025.1.38.0 of the "Free edition" on Windows 11 24H2 to see what digital artifacts it leaves behind. For now, I'll be using the default installation using the installer and not the standalone version.

## File System Artifacts
Right off the bat, a folder is created in %localappdata% called "Devolutions" that contains a subfolder called "RemoteDesktopManager". It contains many interesting files.

`%localappdata%\Devolutions\RemoteDesktopManager`

| File | Location | File Type | Notes |
| --- | --- | --- | --- |
| Connections.db | %localappdata%\Devolutions\RemoteDesktopManager\Connections.db | SQLite Database | This is the default location and file. It contains all the information about connections. More details later. |
| Connections.log | %localappdata%\Devolutions\RemoteDesktopManager\Connections.log | Text File | Appears to mirror log information in the Connections.db file. |
| RemoteDesktopManager.cfg | %localappdata%\Devolutions\RemoteDesktopManager\RemoteDesktopManager.cfg | Text File | Configuration Settings. |
| Mru.xml | %localappdata%\Devolutions\RemoteDesktopManager\[GUID]\Mru.xml | XML File | Contains most recently used connections. |
| Favorites.xml | %localappdata%\Devolutions\RemoteDesktopManager\[GUID]\Favorites.xml | XML File | Contains favorited connections. |

Utilizing DB Browser for SQLite, I take a look at the tables and information that exist within the `Connections.db` file. I will delve into this database deeper once I get a chance to generate some more realistic test data. The below screenshot shows the table names and I will talk a little bit about a few of them below. More details to come in later posts. 

![Tables](/images/RemoteDesktopManager/dbtables.png)

### Attachment

A user can attach files to connections and this table stores Metadata and the file itself in a BLOB. In the screenshot below, you can see the attachment of the FTK Imager installer to a specific ConnectionID. 

![Attachments](/images/RemoteDesktopManager/attachments.png)

### ConnectionHandbook / ConnectionHandbookHistory

A user can also store notes about connections and this table stores the Metadata and the note itself which can be written with Markdown. The related History table will store historical notes as one might guess.

### Connections

This table stores all the connections that are saved by the user in Remote Desktop Manager. 