---
layout: post
title: Diving Deep - LevelDB Part 2
author: 'ogmini'
tags:
 - LevelDB
 - exploration
---

Continuing from an earlier [https://ogmini.github.io/2025/01/21/Deep-Dive-LevelDB.html](https://ogmini.github.io/2025/01/21/Deep-Dive-LevelDB.html), I've had a chance to quickly try out two different tools on LevelDB databases used by the ChatGPT Desktop App.

- [https://github.com/cions/leveldb-cli](https://github.com/cions/leveldb-cli)
- [https://github.com/mdawsonuk/LevelDBDumper](https://github.com/mdawsonuk/LevelDBDumper)

These two authors should work together as each of their tools has complementary features that the other is missing. 

leveldb-cli is able to handle the IndexedDB idb_cmp1 comparator and LevelDBDumper does not currently have this functionality. It is on their radar though [https://github.com/mdawsonuk/LevelDBDumper/issues/16](https://github.com/mdawsonuk/LevelDBDumper/issues/16). Maybe something I could help with, I'd have to learn Go. Alternatively, refactor everything into C#.

## leveldb-cli

Running the following command:

`leveldb -d "XXXXX\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb" -i dump`

Dumps out all the messages from the chat with ChatGPT along with a load of other information. I haven't had a chance to explore what the other information is. I'll have to cross reference the research done by CCL Solutions Group. What is interesting, it is very obvious that ChatGPT uses Markdown to handle the formatting for the messages. 

![leveldb-cli output](/images/leveldb/leveldb-cli-1.png)

## LevelDBDumper

Running the following command:

`LevelDBDumper.exe -d "XXXXX\OpenAI.ChatGPT-Desktop_2p2nqsd0c76g0\LocalCache\Roaming\ChatGPT\IndexedDB\https_chatgpt.com_0.indexeddb.leveldb"`

Unfortunately results in no information besides the following message, "IndexedDB idb_cmp1 comparator not yet implemented, results will not be output".

Still more to digest and taking this one bite at a time. Probably won't get very far by Friday for the Sunday Funday challenge. But, we have a plan and positive, demonstrable  results!




