---
layout: post
title: Remote Desktop Manager - Artifacts Part 7
author: 'ogmini'
tags:
 - DFIR
 - Remote Desktop Manager
---

The 2025 NYS Cybersecurity Conference has been great so far and there is still one more day of presentations. I'm going to digest everything a little bit before posting my thoughts and experiences. Instead, I'm going back to Remote Desktop Manager and the ability to store attachments or files to a connection. Again, maybe useful for storing a script or some other file. The `Connections.db` stores the information related to this artifact in the Attachment table.

Some specific columns that are useful/interesting:

| Column | Description |
| --- | --- |
| ID | Unique ID for this attachment |
| Description | User provided description for the attachment |
| ConnectionID | Unique ID of the related Connection |
| CreationDate | Shows the creation datetime for the recrod. |
| CreationDateTime | Oddly, shows the last modified datetime for the record. |
| Username | Who created the record. |
| Data | XML information about the attachment. Pay attention to this as some information only resides in here |
| AttachmentData | Encrypted binary data of the attached file. |
| FileSize | Oddly, larger than actual size. |

Lot of oddities in this record... Starting with the first one that ModifiedDateTime is stored as CreationDateTime. Why? Who knows; but testing proves it. Funny tidbit, this reminds me of a mistake I made naming a database column as "timestampe" instead of "timestamp". It somehow made it past all the reviews and still lives today...

The XML data stored in the Data column should not be glossed over as it contains some "missing" information from the columns. 

- ModifiedDate is also stored here and named appropriately. 
- ModifiedUsername/ModifiedLoggedUsername lets us know who last modified the attachment record. 
- Filename provides the original filepath of the added file
- SafePassword is an optional field that stores an encrypted password that was set by the user. This doesn't appear to have any security implications on the actual attachment. You do not need to provide it to view the attachment and it doesn't alter the AttachmentData column. 
- Size provides the size in bytes of the original file. This does match unlike FileSize.
- Title is the user provided name for this attachment.

The AttachmentData column at the moment has me stumped. I'm pretty sure its encrypted in some fashion as when I add the same exact file multiple times, I end up with different values. This will probably require some reverse engineering to figure out. 

The discrepancy in the FileSize is also interesting but possibly related to the encryption? The increase is not static but does appear to be related to the original file size. 

Areas to explore for the future...