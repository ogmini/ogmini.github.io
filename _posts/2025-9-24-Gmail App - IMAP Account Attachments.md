---
layout: post
title: Gmail App - IMAP Account Artifacts (Attachments)
author: 'ogmini'
tags:
 - Android
 - ALEAPP
---

Now that I have a test Pixel 7 that has been rooted, I'm starting to dig into some Android forensics. Following from a previous [post](https://ogmini.github.io/2025/08/20/ALEAPP-Plugin-Gmail-App-IMAP-Accounts.html), I'm still looking at how third party IMAP Accounts are handled in the default Gmail application on Android 16. The below post is very in the moment so take everything with a grain of salt. My [PR](https://github.com/abrignoni/ALEAPP/pull/590) to ALEAPP was merged and I intend on improving it a bit as it resulted from artifacts from a CTF. There are plently more avenues to explore and document.

## Testing Setup

Pixel 7
Android 16 BP3A.250905.014

Gmail Application Version: 2025.09.08.806471915.Release

Using adb, I open a shell to the Pixel 7 and use the `su` command to elevate permissions to root. The `tar` command allows me to tar up a directory in order to save it to my computer for further examination.

~~~ bash
su 
tar -cvf /storage/self/primary/Download/gm.tar /data/data/com.google.android.gm/
~~~

`adb pull` lets me download the tar file to my computer

~~~ cmd
adb pull /storage/self/primary/Download/gm.tar
~~~

You may decide to save the tar file to a different location. The above would need to be changed appropriately.

## Email Attachments

I wanted to see how email attachments are handled and what digital artifacts they leave behind. I created two test accounts on [runbox.com](runbox.com) (This is not an endorsement of the service or company) and connected both accounts to the Gmail Application. I also had a third Google Account associated with the phone. This left me with three email accounts associated with the Gmail Application and I sent a few test emails back and forth between the accounts. Some of these emails included image and pdf attachmets.

I already know from previous research that `\data\com.google.android.gm\` contains various files that are associated with the Gmail Application.

### EmailProviders.db

This database is located in `\data\com.google.android.gm\databases` and the following tables are relevant for our current discussion:

- Account - Contains details about the IMAP Accounts
- Attachment - Contains details about attachments including an identifier pointing back to a record in the Message table
- Message - Contains records about IMAP emails

Below is a quick database diagram and the relations between the tables. I have removed columns that are not relevant to the current discussion.

![Database Tables](/images/gmailimap/db-attachment.png)

A quick SQL query that would give you all attachments and their associated emails and accounts.

~~~ sql
SELECT * FROM Account AS A 
INNER JOIN Message AS M ON A._id = M.AccountKey
INNER JOIN Attachment AS AM ON M._id = AM.messageKey
~~~

### Attachment Files

To locate and retrieve any of the attachments, two pieces of information are needed from the database:

1. Account._id - Referred to as {Account#}
2. Attachment._id - Referred to as {Attachment#}

The attachment files are located at `\data\com.google.android.gm\databases\{Account#}.db_att\{Attachment#}`. An example path would be `\data\com.google.android.gm\databases\1.db_att\3` for an Account ID of 1 and an Attachment ID of 3.

The file has no extension but the fileName, mimeType, and size is known from the Attachment table. Just adding the appropriate file extension will allow the file to be open normally.

## Next Steps

- Incorporate this into the existing ALEAPP Plugin.
- Explore and understand more of the database structure and data.
- Ultimate plan is to consolidate and write up all of this information.

## References

- [https://android.googlesource.com/platform/packages/apps/Email/+/eclair-release/src/com/android/email/provider/AttachmentProvider.java](https://android.googlesource.com/platform/packages/apps/Email/+/eclair-release/src/com/android/email/provider/AttachmentProvider.java)
