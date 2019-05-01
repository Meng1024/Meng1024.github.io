---
layout:     post
title:      "[Designing Data-Intensive Applications] Chapter 2"
subtitle:   "Chapter 2"
categories: System-Design
date:       2019-04-30
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - System Design
---

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
