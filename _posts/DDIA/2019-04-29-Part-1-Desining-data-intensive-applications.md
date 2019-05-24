---
layout:     post
title:      "[Designing Data-Intensive Applications] Part 1"
subtitle:   "Chapter 1"
categories: System-Design
date:       2019-04-29
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - System Design
---

## Chapter 1
据小伙伴说，这本书去年还挺火的Designing Data-Intensive Applications.
最近闲来无聊读读看看。

前言加第一章一般都是预热章节， 给一个high level idea. 作者聊聊这书给谁看的。挺有意思的。
第一章就是讲什么是 Reliability，Scalability，Maintainability

放上chapter章尾的summay：

1. Reliability means making systems work correctly, even when faults occur. Faults can be in hardware (typically random and uncorrelated), software (bugs are typically sys‐ tematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

2. Scalability means having strategies for keeping performance good, even when load increases.

3. Maintainability 就是说好好写代码，好好维护。。。


## Chapter 2
这章主要讲了3种database 类型

NoSQL
1. Document databases target use cases where data comes in self-contained documents and relationships between one document and another are rare.
2. Graph databases go in the opposite direction, targeting use cases where anything is potentially related to everything.

SQL
3. Relation database

这有一个[youtube video](https://www.youtube.com/watch?v=ZS_kXvOeQ5Y) 对比NoSQL和SQL.简单明了。 图片也是摘自该视频。
当需要存储大量多对多的关系，SQL更新起来比较方便，NoSQL这种情况下可能需要重复更新一些值。但是如果是大量的读操作，如果SQL需要join很多表的话，那么这时候NoSQL可能更好一点。 所以不能简单说哪种类型的数据库更好， 需要看被处理的数据类型和load。

![here](https://meng1024.github.io/images/posts/system_design/databaseCompare.png)



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


## Chapter 4
This chapter discussed several data encoding formats and their compatibility properties and several modes of dataflow.

### Data encoding formats

1. Programming language–specific encodings are restricted to a single program‐ ming language and often fail to provide forward and backward compatibility.

2. Textual formats like JSON, XML, and CSV are widespread, and their compatibility depends on how you use them. e.g. JSON and XML have good support for Unicode character strings, but they don’t support binary strings. So people get around this limitation by encoding the binary data as text using Base64.

3. Binary schema–driven formats like Thrift, Protocol Buffers, and Avro allow compact, efficient encoding with clearly defined forward and backward compatibility semantics. The schemas can be useful for documentation and code genera‐ tion in statically typed languages. However, they have the downside that data needs to be decoded before it is human-readable.

### Dataflow
1. Databases
2. RPC and REST APIs
 - The RPC model tries to make a request to a remote net‐ work service look the same as calling a function or method in your programming language, within the same process. 这里书里罗列了几点local requests和网络之间call的不同.
3. Message-Passing Dataflow: a difference compared to RPC is that message-passing communication is usually one-way: a sender normally doesn’t expect to receive a reply to its messages.

这章看起来还是比较轻松的，几乎没讲什么。都是一些基础概念。。过
