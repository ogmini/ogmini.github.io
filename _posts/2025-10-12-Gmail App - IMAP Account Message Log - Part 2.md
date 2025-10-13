---
layout: post
title: Gmail App - IMAP Account Artifacts (Message Logging) - Part 2
author: 'ogmini'
tags:
 - Android
 - ALEAPP
---

This post will show why focused, repeatable testing is so important. Yesterday's [post](https://ogmini.github.io/2025/10/11/Gmail-App-IMAP-Account-Message-Log-Part-1.html) hinted at some possible message logging tables in one of the sqlite databases. I had come across entries while testing for attachments that I had not seen previously. Initially, I had chalked that up to not having sent any emails to the Trash folder until I wanted to test what happened to cached attachments. Well...

I attempted some testing focused just on moving an email to the Trash folder. The steps were:

1. "Delete" email
2. Go to Trash Folder and verify email is there
3. Go back to Home Screen of phone
4. ADB Pull database

When I pulled the sqlite database from the phone and examined the two tables, I saw no records! Obviously, I must have made a mistake when running my test so I ran it again and got the same results. I did not run the same test again as that would truly be the mark of insanity. The question became what did I do differently when testing for the cached attachments? It took me a bit to figure this one out as I had to think of different testing steps as my notes from the attachment testing were not detailed enough in this case. It turns out that the following steps will give us the results we are looking for:

1. "Delete" email
2. Go to Trash Folder and verify email is there
3. ADB Pull database

Just the act of going back to the Home Screen of the phone causes those logging records to go poof. My best guess/theory is that those records are transient and only present to support that "Undo" option that will pop up on the bottom of the screen. 

![Undo Option](/images/gmailimap/undo-option.png)

I don't want to say that going back to the Home Screen is the trigger for the cleanup as it could be some other process that just happens to be called when you exit out of the Gmail App. Not sure it is worth the time to figure out the exact mechanism that triggers the clenaup. With this information, I'm very tempted to say that this piece of knowledge is pretty useless. You'd literally need to be grabbing the artifact from a live phone before the Gmail App had a chance to flush the records. In a real word case, I don't see that opportunity presenting itself and I was hoping for records that were not so transient. 