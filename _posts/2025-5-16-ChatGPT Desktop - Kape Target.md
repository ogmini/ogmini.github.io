---
layout: post
title: ChatGPT Desktop - Kape Target
author: 'ogmini'
tags:
 - DFIR
 - ChatGPT 
 - KAPE
---

Just submitted a Target to KapeFiles for ChatGPT Desktop. I realized one didn't exist and I had done [research](https://ogmini.github.io/2025/01/20/David-Cowen-Sunday-Funday-ChatGPT.html) into its artifacts for one of David Cowen's Sunday Funday challenges. It will grab:

- various Application Registry Hives
    - `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\Settings`
    - `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\SystemAppData\Helium`
- LevelDB and IndexedDB files
    - `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb`
    - `%localappdata\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Local Storage\leveldb`
- ChromeCache files
    - `%localappdata%\Packages\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\Cache`

You will need to use tools such as [https://github.com/cions/leveldb-cli](https://github.com/cions/leveldb-cli) to examine the LevelDB/IndexedDB files. 