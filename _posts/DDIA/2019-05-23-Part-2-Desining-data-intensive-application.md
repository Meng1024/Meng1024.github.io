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

### Main approaches to replication:
1. Single-leader replication
2. Multi-leader replication
3. Leaderless replication
这三种类型各有利弊，主要是考虑数据之间如何copy.

### A few consistency models
1. Read-after-write consistency: Users should always see data that they submitted themselves.
2. Monotonic reads: After users have seen the data at one point in time, they shouldn’t later see the data from some earlier point in time.
3. Consistent prefix reads: Users should see the data in a state that makes causal sense: for example, seeing a question and its reply in the correct order.
