---
layout: post
title: David Cowen Sunday Funday Challenge - ChatGPT Desktop Artifacts
author: 'ogmini'
tags:
 - sunday funday
 - challenge
---

IN PROGRESS

## Test Setup

I am performing testing/validation using a Hyper-V VM running Windows 11 Pro 24H2 OS Build 26100.2894.

ChatGPT Desktop Details:
> name: ChatGPT  
> version: 1.2025.16  
> locale: en-US  
> os_name: Windows_NT  
> os_version: 10.0.26100  
> arch: x86_64  

The following tools are being used:

- FTK Imager 4.7.3.81
- Process Monitor
- Strings
- Registry Explorer
- 010 Editor
- DB Browser for SQLite
- ChromeCacheView
- ChromeHistoryView
- LevelDBDumper
- leveldb-cli


### Steps

1. Install ChatGPT via the Windows Store
2. Sign into ChatGPT
3. ~~Change settings~~ TODO
    - My GPTs
    - Customize ChatGPT
    - Settings
4. Perform various actions 
    - Start Chat
    - Attach File
    - ~~Use voice mode~~ TODO
    - ~~Temporary Chat Flag~~ TODO

At each step of the way, we will monitor for changes to the system using Process Monitor. The `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0` folder will also be copied using FTK Imager for further analysis and comparison. 

## Preliminary Analysis

This is a standard Electron App installed by the Windows App Store that contains the standard:

- a `setting.dat` file just like I described in my research into Notepad [https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings](https://github.com/ogmini/Notepad-State-Library?tab=readme-ov-file#settings). 
- a Helium container folder holding User and UserClasses registry hives. 
- various SQLite databases
- various leveldb databases

Initial search using [strings](https://learn.microsoft.com/en-us/sysinternals/downloads/strings) is unable to find anything related to login of "ogmini". We might need to look into the browser's artifacts to find anything. Maybe they have a saved credential in the browser.

~~~
strings -s "%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0" | findstr /i "ogmini"
~~~

Strings does find files containing words that were part of a chat I had with ChatGPT on "snow" in the following locations:  

- `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb`
- `%localappdata\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Local Storage\leveldb`

~~~
strings -s "%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0" | findstr /i "snow"
~~~

Both of these are LevelDB databases. They also appear to log both sides of the conversation. Have to find a tool that can view/parse these into something more structured and readable. Viewing some of the files with 010 Editor makes it very obvious that conversations are contained within. 

Preliminary glimpses at the registry hives using RegistryExplorer do not find anything of note. This isn't too unexpected as I haven't made any changes to settings or defaults and these might not even be utilized by the application. If this behaves similarly to Windows Notepad, registry records will only be created when the default value for a setting is changed. 

Registry Hives exist in the following locations:

- `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\Settings`
- Helium Container - `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\SystemAppData\Helium`

## Deeper Analysis

### Login / Authentication Artifacts

[Detailed Post](https://ogmini.github.io/2025/01/23/ChromeCacheView-and-ChromeHistoryView.html)

[Video of Authentication](https://youtu.be/MMvABslvhIs)

Using ChromeCacheView, I examine `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Cache` and find records related to authentication highlighted in the screenshots below.

![ccview](/images/chatgpt/chromecacheview-login.png)

![authview](/images/chatgpt/chromecacheview-startuppage.png)

Using ChromeHistoryView, I examine the history for the Edge Browser which is currently set as the default browser. Records related to the actual authentication process with OpenAI can be seen starting at 9:58:40 AM which again lines up with our test scenario. These records culminate with the prompt to open the ChatGPT Desktop App at 9:59:13 AM after successful authentication. Again, they have been highlighted in the screenshot below.

![chview](/images/chatgpt/chromehistoryview-login.png)

#### Timeline

Using the above artifacts, we can construct a generalized timeline of actions as below.

| Timestamp | Source | Description |
| --- | --- | ---|
| 9:58:05 AM | ChatGPT Cache | Application launched and login.htm page displayed |
| 9:58:40 AM | Edge History | ChatGPT Login Page loaded |
| 9:58:54 AM | Edge History | Account Authorized |
| 9:59:13 AM | Edge History | Callback to open ChatGPT Desktop App |
| 9:59:23 AM | ChatGPT Cache | Callback to open ChatGPT Desktop App |
| 9:59:24 AM | ChatGPT Cache | Startup Page loaded |

### Chat Message Artifacts

[Detailed Post](https://ogmini.github.io/2025/01/22/Deep-Dive-LevelDB-Part-2.html)

[Video of Chatting](https://youtu.be/Q0dW4CE9WQs)

Using leveldb-cli and running the following command:

~~~
leveldb -d "XXXXX\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb" -i dump
~~~

Dumps out all the messages from the chat with ChatGPT along with a load of other information. I haven't had a chance to explore what the other information is. I'll have to cross reference the research done by CCL Solutions Group. What is interesting, it is very obvious that ChatGPT uses Markdown to handle the formatting for the messages. 

![leveldb-cli output](/images/leveldb/leveldb-cli-1.png)

### File Upload Artifacts

[Detailed Post](https://ogmini.github.io/2025/01/23/ChromeCacheView-and-ChromeHistoryView.html)

[Video of File Upload](https://youtu.be/0noorxQagPU)

I started a new conversation topic by uploading a screenshot and asking ChatGPT to tell me about it. Using leveldb-cli in a similar fashion to yesterday's [post](https://ogmini.github.io/2025/01/22/Deep-Dive-LevelDB-Part-2.html), I see the following text:

`[media pointer="file-service://file-QovPq6QZ7EK4CrJzMjxN7S"]`

![leveldb-cli](/images/chatgpt/leveldb-cli-fileupload.png)

This led me to once again, use strings to search for "QovPq6QZ7EK4CrJzMjxN7S" and I got a few hits and most importantly hits within the ChromeCache.

![strings](/images/chatgpt/strings-fileupload.png)

`1/0/https://files.oaiusercontent.com/file-QovPq6QZ7EK4CrJzMjxN7S?se=2025-01-23T17%3A58%3A27Z&sp=r&sv=2024-08-04&sr=b&rscc=max-age%3D299%2C%20immutable%2C%20private&rscd=attachment%3B%20filename%3Dchromecacheview-startuppage.png&sig=rsli2NzsWVBG%2B/P4R%2BUnW7GSrvCVEyaxoxWwhiRp5xA%3D`

Zipping over to ChromeCacheView, I see a handy record that links "file-QovPq6QZ7EK4CrJzMjxN7S" to "chromecacheview-startuppage.png". Right-clicking and choosing "Open Selected Cache File" even retrieves the image for us. In this case, the file is stored in the f_00001d data stream file and renaming the file to *.png allows us to open the file manually.  

![chuploadview](/images/chatgpt/chromecacheview-fileupload.png)

## Future Research

- Read more into the LevelDB / IndexedDB to fully understand the format. 
- Improve existing tooling or write new ones to more easily understand the data in the LevelDB / IndexedDB files. 
- Explore the other scenarios of changing settings, temporary chat flag, and voice mode.

## Conclusion

Artifacts related to the chat messages and uploaded files can be recovered using a combination of ChromeCacheView and leveldb-cli. 

Artifacts related to login can be recovered using ChromeCacheView and using other tools to examine the history of the browser used for authentication. These artifacts can help place a timeline for when the user signed into the service. 



