---
layout:     post
title:      "[Designing Data-Intensive Applications] Chapter 3"
subtitle:   "Chapter 3"
categories: System-Design
date:       2019-05-13
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - System Design
---

## Chapter 3
这章主要讲数据的存储与索引：

### 数据库索引
#### Hash indexes

  1. what is hash indexes?  
Keep an in-memory hash map where every key is mapped to a byte offset in the data file—the location at which the value can be found

  2. use case : Bitcask (the default storage engine in Riak)

  3. how do we avoid eventually running out of disk space?
   - compaction: merge several segments into one and discard the old data.

  4. advantages:

     - Appending and segment merging are sequential write operations, which are gen‐ erally much faster than random writes

     - oncurrency and crash recovery are much simpler if segment files are append- only or immutable.

     - Merging old segments avoids the problem of data files getting fragmented over time.

  5. disadvantages:

      - The hash table must fit in memory, not good if we have a very large number of keys

      - Range queries are not efficient. need to scan each key individually.

为了改进以上的缺点，整了个LSM tree 和B tree出来。
首先我们需要看一下什么叫 Sorted String Table(SSTable) : sorted key-value pairs
和其优点（相对于无序追加）:
  - Merging segments is simple and efficient
  - Don't need to keep an index of all the keys in memory since it is sorted by key.
  - compression data first and wirte to dish will reduce I/O bandwidth use

#### LSM-tree
To be continued...
#### B Tree
