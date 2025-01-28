---
layout: post
title: Diving Deep - LevelDB Part 3
author: 'ogmini'
tags:
 - LevelDB
 - exploration
---

Continuing my analysis of the ChatGPT Desktop App by creating binary template files to help me understand the LevelDB and IndexedDB databases. I personally find it useful to "visually" see the binary files structured in hex editors like 010 Editor and ImHex. It helps me know if I'm on the right track and it is pretty easy to later convert that to code for a tool. 

I'll be starting with the easier LevelDB .log files and work my way up in complexity. Alex Caithness over at CCL Solutions Group has a great [article](https://www.cclsolutionsgroup.com/post/hang-on-thats-not-sqlite-chrome-electron-and-leveldb) on the format and I worked from that as a starting point. 

I created the binary template file for the standard LevelDB .log files. The file needs some cleanup and I haven't yet had the chance to create a GitHub Repository to hold them. I'll post it below, but it would be wise to check my GitHub for an updated/clean version at a later date. I've started on the binary template file for the LevelDB .ldb files. This one is a little more complex and I've run out of steam for the day. 

The IndexdDB files are a different beast all together. Again, I'll be leveraging prior research by [Alex Caithness](https://www.cclsolutionsgroup.com/post/indexeddb-on-chromium).

## Preliminary Binary Template

```
//------------------------------------------------
//--- 010 Editor v15.0.1 Binary Template
//
//      File: LevelDB-Log.bt
//   Authors: ogmini
//   Version: 0.1
//   Purpose: Template to make sense of LevelDB .log files.
//  Category: Database
// File Mask: *.log
//  ID Bytes: 
//   History: 
//   0.1    2025-1-27 ogmini: Initial Version
//------------------------------------------------

//------------------------------------------------
//                   Structs
//------------------------------------------------

typedef struct {
    do {
        ubyte bytes;
    } while (bytes > 0x7f);
} varint32le <read=varint32leValueToStr>;

//------------------------------------------------
//                   Funcs
//------------------------------------------------

int32 Decodevarint32le(varint32le &varint) {
    local int32 val = 0;
    local int i;
    local int32 num;
    for( i = 0; i < sizeof(varint); i++ ) {
        num = varint.bytes[i] & 0x7F;
        val |= num << (i * 7);
    }
    return val;
}

string varint32leValueToStr(varint32le &varint) {
    return Str("%Lu", Decodevarint32le(varint));
}

typedef struct {
    ubyte recordstate <bgcolor=cRed, comment="TODO">;
        varint32le key_length <bgcolor=cGreen>;
        char key[Decodevarint32le(key_length)] ;
        varint32le value_length <bgcolor=cDkGreen>;
        char value[Decodevarint32le(value_length)];
    } srec <bgcolor=cYellow>;
    
typedef struct {
    struct  blockheader {
        uint32 crc32 <format=hex, bgcolor=cLtGray, comment="TODO">;
        int16 datalength <bgcolor=cGray>;
        ubyte blocktype <bgcolor=cDkGray>;
    } header;
    
        struct batchheader {
        int64 sequencenumber <bgcolor=cBlue>;
        int32 recordcount <bgcolor=cDkBlue>;
    } batch ;
    
     srec records[batch.recordcount] <optimize=false>; 
    
    } bl;

local int i; 

 while (!FEof())
{
bl block;     
}
```

