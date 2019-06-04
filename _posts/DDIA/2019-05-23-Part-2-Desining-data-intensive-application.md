---
layout:     post
title:      "[Designing Data-Intensive Applications] Part 2"
subtitle:   "Chapter 5"
categories: System-Design
date:       2019-05-23
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - System Design
---


### Chapter 5
哇，这章也基本全是概念。抄一个summary吧。
### why Replication

1. High availability: Keeping the system running, even when one machine (or several machines, or an entire datacenter) goes down
2. Disconnected operation: Allowing an application to continue working when there is a network interruption
3. Latency: Placing data geographically close to users, so that users can interact with it faster
4. Scalability: Being able to handle a higher volume of reads than a single machine could handle, by performing reads on replicas

#### Main approaches to replication:
1. Single-leader replication
2. Multi-leader replication
3. Leaderless replication
这三种类型各有利弊，主要是考虑数据之间如何copy.

#### A few consistency models
1. Read-after-write consistency: Users should always see data that they submitted themselves.
2. Monotonic reads: After users have seen the data at one point in time, they shouldn’t later see the data from some earlier point in time.
3. Consistent prefix reads: Users should see the data in a state that makes causal sense: for example, seeing a question and its reply in the correct order.

### Chapter 6
Partitioning(sharding) is a way of intentionally breaking a large database down into smaller ones. The main reason for wanting to partition data is scalability

If the partitioning is unfair, it can cause:
- hot spots (nodes with disproportionately high load)
- skewed (some partitions have more data or queries than others)

#### Partitioning
Different ways of partitioning a large dataset into smaller subsets:

1. Partitioning by Key Range
  - advantages: easy for range queries
  - diadvantages: certain access patterns can lead to hot spots. e.g. if index is timestamp. all data in a timespan will go to the same partition.

  In this approach, partitions are typically rebalanced dynamically by splitting the range into two subranges when a partition gets too big.

2. Partitioning by Hash of Key
  - advantages: reduce hot spots but it cannot avoid entirely. e.g. a celebrity user with millions of followers 发生点什么新闻， 用户评论讲写入同一个key. 一个简单的解决办法，在key后面加一些random number. 但是这样做的话，read的时候需要额外的合并操作.
  - disadvantages: lose a nice property of key-range partitioning

  When partitioning by hash, it is common to create a fixed number of partitions in advance, to assign several partitions to each node, and to move entire parti‐ tions from one node to another when nodes are added or removed. Dynamic partitioning can also be used.

#### Partitioning and Secondary Indexes

- Document-partitioned indexes (local indexes), where the secondary indexes are stored in the same partition as the primary key and value. This means that only a single partition needs to be updated on write, but a read of the secondary index requires a scatter/gather across all partitions.

- Term-partitioned indexes (global indexes), where the secondary indexes are partitioned separately, using the indexed values. An entry in the secondary index may include records from all partitions of the primary key. When a document is written, several partitions of the secondary index need to be updated; however, a read can be served from a single partition.
