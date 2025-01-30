---
layout: post
title: Diving Deep - LevelDB Part 4
author: 'ogmini'
tags:
 - LevelDB
 - exploration
---

Continuing work on the binary template file for the LevelDB .ldb files. Learning a lot and pushing my knowledge boundaries. I am definitely recreating prior research; but I find this is the best way to learn and also validate previous findings. It is also possible things have changed.

I'm leaning heavily at looking at the golang and C++ implementations of LevelDB on Google's various GitHub repositories. The following links have been useful:

- https://github.com/golang/leveldb/blob/master/table/table.go
- https://github.com/google/leveldb/blob/main/doc/table_format.md
- https://github.com/google/leveldb/blob/main/table/block_builder.cc
- https://www.cclsolutionsgroup.com/post/hang-on-thats-not-sqlite-chrome-electron-and-leveldb

I still haven't made much progress due to time constraints. At the moment, I have implemented the structure for:

- BlockHandles
- BlockTrailers

I have also been able to mark and locate the following:

- Index Block Handle
- Index Block  
- Meta Index Block Handle
- Meta Index Block
- Magic Bytes/File Signature

I'm trying to wrap my head around how the Block and BlockEntry structures work. I'll also have to tackle the compression. 