---
layout: post
title: ChromeCacheView / ChromeHistoryView 
author: 'ogmini'
tags:
 - Chrome
 - Exploration
---

Continuing my [work](https://ogmini.github.io/2025/01/20/David-Cowen-Sunday-Funday-ChatGPT.html) on David Cowen's Sunday Funday challenge, I leveraged ChromeCacheView and ChromeHistoryView to look both at the Edge Browser and the ChatGPT Desktop App. I want to see if we can capture the user authentication process with timestamps and any artifacts related to uploaded files.

## User Authentication

### ChromeCacheView

The cache for the ChatGPT Desktop App is examined using ChromeCacheView. Typically the location for this will be `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Cache`.

Some records related to authentication can be seen starting at 9:58:05 AM which lines up with our test scenario.  

![ccview](/images/chatgpt/chromecacheview-login.png)

Subsequently, a record at 9:59:24 AM shows us the start page for the ChatGPT Desktop App being loaded. This lines up with the successful authentication being passed back to the ChatGPT Desktop App.

![authview](/images/chatgpt/chromecacheview-startuppage.png)

The actual authentication process can be see using ChromeHistoryView in the next section.

### ChromeHistoryView

Next, I looked at the History for the Edge Browser which is currently set as the default browser.

Some records related to the authentication can be seen starting at 9:58:40 AM which again lines up with our test scenario. These records culminate with the prompt to open the ChatGPT Desktop App at 9:59:13 AM after successful authentication.

![chview](/images/chatgpt/chromehistoryview-login.png)

### Timeline

| Timestamp | Source | Description |
| --- | --- | ---|
| 9:58:05 AM | ChatGPT Cache | Application launched and login.htm page displayed |
| 9:58:40 AM | Edge History | ChatGPT Login Page loaded |
| 9:58:54 AM | Edge History | Account Authorized |
| 9:59:13 AM | Edge History | Callback to open ChatGPT Desktop App |
| 9:59:23 AM | ChatGPT Cache | Callback to open ChatGPT Desktop App |
| 9:59:24 AM | ChatGPT Cache | Startup Page loaded |

## File Upload

I started a new conversation topic by uploading a screenshot and asking ChatGPT to tell me about it. Using leveldb-cli in a similar fashion to yesterday's [post](https://ogmini.github.io/2025/01/22/Deep-Dive-LevelDB-Part-2.html), I see the following text:

`[media pointer="file-service://file-QovPq6QZ7EK4CrJzMjxN7S"]`

![leveldb-cli](/images/chatgpt/leveldb-cli-fileupload.png)

This led me to once again, use strings to search for "QovPq6QZ7EK4CrJzMjxN7S" and I got a few hits and most importantly hits within the ChromeCache.

![strings](/images/chatgpt/strings-fileupload.png)

`1/0/https://files.oaiusercontent.com/file-QovPq6QZ7EK4CrJzMjxN7S?se=2025-01-23T17%3A58%3A27Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Dchromecacheview-startuppage.png&sig=rsli2NzsWVBG%2B/P4R%2BUnW7GSrvCVEyaxoxWwhiRp5xA%3D`

Zipping over to ChromeCacheView, I see a handy record that links "file-QovPq6QZ7EK4CrJzMjxN7S" to "chromecacheview-startuppage.png". Right-clicking and choosing "Open Selected Cache File" even retrieves the image for us. In this case, the file is stored in the f_00001d data stream file and renaming the file to *.png allows us to open the file manually.  

![chuploadview](/images/chatgpt/chromecacheview-fileupload.png)
