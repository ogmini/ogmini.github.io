---
layout: post
title: Remote Desktop Manager - Artifacts Part 4
author: 'ogmini'
tags:
 - DFIR
 - Remote-Desktop-Manager
---

Quick post today as it is the start of the Memorial Day weekend! Remote Desktop Manager has the ability to add documentation to a connection. Useful for keeping notes, processes, and documentation in a place specifically related to that system. It is similar to a OneNote Notebook. The `Connections.db` stores the information related to this artifact in the following tables:

- ConnectionHandbook
- ConnectionHandbookHistory

As you can imagine the ConnectionHandbook table stores all the various pages of information. Some specific columns that are useful/interesting:

| Column | Description |
| --- | --- |
| ID | Unique ID for this page |
| ConnectionID | Unique ID of the related Connection |
| Name | Name of the page |
| Data | Actual contents of the page |
| CreationDate | When the page was created |
| CreationUsername / CreationLoggedUserName | Who created the page |
| ModifiedDate| When the page was last modified |
| ModifiedUsername / ModifiedLoggedUserName | Who last modified the page | 
| SortPriority | Ordering the pages in the notebook. Starts at 0. |
| IsDefault | Default start page |
| DocumentationType | Supports different types of pages. Markdown = 1, Plain Text = 2, HTML = 3, Form = 4 |

The ConnectionHandbookHistory table stores information about all the changes including deletions. This allows you to see a history of the page over time as chanegs are saved. User's can delete the history by emptying the "recycle bin". Until that point, you can trivially recover that information. If they have emptied the "recycle bin", it is possible to recover some deleted records from the SQLite database with some work. Again, I just want to point out some specific columns and point out behavior.

| Column | Description | 
| --- | --- |
| HistoryType | I = Page Creation, U = Page Update, R = Page Restored from "recycle bin", D = Page Deleted. If a Page is removed from the "recycle" bin all associated records are also deleted. |
| ConnectionHandbookID | ID that relates back to the ID in ConnectionHandbook |
| Name | Name of the page |
| Data | Changed content | 
| ModifiedDate| When the page was modified |
| ModifiedUsername / ModifiedLoggedUserName | Who modified the page | 

