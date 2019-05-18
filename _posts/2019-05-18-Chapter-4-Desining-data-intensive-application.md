---
layout:     post
title:      "[Designing Data-Intensive Applications] Chapter 4"
subtitle:   "Chapter 4"
categories: System-Design
date:       2019-05-18
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - System Design
---

## Chapter 4
 This chapter discussed several data encoding formats and their compatibility properties and several modes of dataflow.

### Data encoding formats

 1. Programming language–specific encodings are restricted to a single program‐ ming language and often fail to provide forward and backward compatibility.

 - Textual formats like JSON, XML, and CSV are widespread, and their compatibil‐ ity depends on how you use them. e.g. JSON and XML have good support for Unicode character strings, but they don’t support binary strings. So people get around this limitation by encoding the binary data as text using Base64.

 - Binary schema–driven formats like Thrift, Protocol Buffers, and Avro allow compact, efficient encoding with clearly defined forward and backward compati‐ bility semantics. The schemas can be useful for documentation and code genera‐ tion in statically typed languages. However, they have the downside that data needs to be decoded before it is human-readable.

### Dataflow
1. Databases
2. RPC and REST APIs
  - The RPC model tries to make a request to a remote net‐ work service look the same as calling a function or method in your programming lan‐ guage, within the same process. 这里书里罗列了几点local requests和网络之间call的不同.
3. Message-Passing Dataflow: a difference compared to RPC is that message-passing communication is usually one-way: a sender normally doesn’t expect to receive a reply to its messages.

这章看起来还是比较轻松的，几乎没讲什么。都是一些基础概念。。过
