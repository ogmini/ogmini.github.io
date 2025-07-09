---
layout: post
title: Diving Deep - LevelDB Part 5
author: 'ogmini'
tags:
 - LevelDB
 - Exploration
---

LevelDB utilizes CRC32 for data integrity checks; but it doesn't use the normal CRC32 algorithm. Reading the code comments it utilizes a masked representation of CRC32 because it is problematic to compute the CRC of a string that contains embedded CRCs. The code accomplishes this by rotating right by 15 bits and adding a constant of `0xa282ead8ul`.

Making some slow progress on this.

<https://github.com/google/leveldb/blob/main/util/crc32c.h>

<https://selfboot.cn/en/2024/08/29/leveldb_source_utils/>

Will have to figure out how to implement this into the Binary Template.
