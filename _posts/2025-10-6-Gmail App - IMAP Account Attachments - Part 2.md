---
layout: post
title: Gmail App - IMAP Account Artifacts (Attachments) - Part 2
author: 'ogmini'
tags:
 - Android
 - ALEAPP
---

Continuing to look at IMAP Account attachments from a previous [post](https://ogmini.github.io/2025/09/24/Gmail-App-IMAP-Account-Attachments.html). Shifting gears for a moment away from looking the the lifecycle of digital photos. I wanted to verify my understanding of the folder structure for how attachments were stored. I had made a "informed" assumption that the attachment files are located at `\data\com.google.android.gm\databases\{Account#}.db_att\{Attachment#}`. Where the Account# and Attachment# come from the various database tables. 

I had initially done my testing with 2 accounts which had Account #s of 1 and 2; however, I did not send any attachments to the second account. Just to make sure the Account# actually corresponded to the ID in the database table. I created a 3rd account and sent emails with attachments to the email address. As expected, the following paths now existed:

- `\data\com.google.android.gm\databases\1.db_att\`
- `\data\com.google.android.gm\databases\3.db_att\`

No path for 2.db_att exists. 

## Sent Attachments

Attachments that are sent from the phone are not stored the same way. In the `Attachment` table you will see records with a non-NULL value for "cachedFile". For example `content://com.google.android.gm.email.provider/attachment/cachedFile?filePath=%2Fdata%2Fuser%2F0%2Fcom.google.android.gm%2Fcache%2F2025-10-06-15%3A49%3A463611689731535944683.attachment`. Breaking that down, the attachment is stored at `\data\com.google.android.gm\cache\2025-10-06-15_49_463611689731535944683.attachment`. More testing is needed to see how long this file will stay in cache. 

The following is also referenced in the contentUri field `content://media/picker/0/com.android.providers.media.photopicker/media/33`. I need to poke around this some more. One would assume this is making reference to my using the photo picker to choose an image to attach. However, the image was not named 33.

I still need to make updates to the ALEAPP Plugin to handle the attachments.
