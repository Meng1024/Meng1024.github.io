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


### OLAP 与 OLTP
OLTP systems are typically user-facing, which means that they may see a huge volume of requests. In order to handle the load, applications usually only touch a small number of records in each query. OLAP are optimized for analytics

### OLAP 数据库索引
#### Column-Oriented Storage 列存储
中间讲了列压缩

### OLTP 数据库索引
#### Hash indexes

  1. Introduce hash indexes
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
1. Introduce LSM: LSM is short for Log-Structured Merge-Tree.
  这有一个[blog](https://blog.csdn.net/sdulibh/article/details/79630614)讲的很清楚。 写的大致原理就是，它 consists of two MemTables
in main memory and a set of SSTables. 当有数据来的时候，先放入log file 中，然后到memTable. 当memTable满了或者定期刷到一个read only Immutable MemTable. 后台线程在刷入level 0. 此时level 0 可能存在重复元素。除此之外的level都不会存在。 每当level l超出了size limit，后台线程就会merge l 层和l+1 层，产生一个新的l+1 层.
读的flow：先查memtable，如果没有，从level0开始一直查下去. 如果该值不存在的话，读的效率会很低，所以一般会用到bloom filter来加速

![lsm](https://meng1024.github.io/images/posts/system_design/LSM.png)

2.  use case: levelDB, RocksDB
3.  advantages
  - support high write throughtput.
  - support range queries.
4. disadvantages
   - read might slow
   - compaction process can sometimes interfere with the performance of ongoing reads and writes
   - If write throughput is high and compaction is not configured carefully, it can happen that compaction cannot keep up with the rate of incoming writes

#### B Tree

1. Introduce B tree: B-trees break the database down into fixed-size blocks or pages, traditionally 4 KB in size (sometimes bigger), and read or write one page at a time. 感觉就是一个多叉树，叶子节点存放的是page. 这个结构里面，写入操作是overwrite的， 因为中间牵扯到page split， 所以写的时候，需要额外一个 WAL（a write-ahead log), 以防写在一半的时候，数据库crash。 这只是其中一种解决办法

2. use case： SQL
3. advantages
 - faster for read operations
 - Each key exists in exactly one place in the B tree, whereas a log-structured storage engine may have multiple copies of the same key in different segments.
4. disadvantages
 - writes slow since overwrite data
