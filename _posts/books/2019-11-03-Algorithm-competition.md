---
layout:     post
title:      "挑战程序设计竞赛"
subtitle:   "leetcode"
categories: LeetCode
date:       2019-11-03
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - Algorithm
---

# Chapter 1
## 轻松热身
1. leetcode 611
2. Ants
描述：一根棍长L，上面有n只蚂蚁，方向不明，速度一致，问最少最多需要多久蚂蚁能全爬下木棍。蚂蚁相碰的时候会掉向。
这个是个脑筋急转弯，因为蚂蚁的速度一样快，所以可以忽略相撞的情形。
3. 4Sum


# Chapter 2
## 2.1 最基础的穷竭搜索
### 2.1.1
斐波那契数列
### 2.1.2 栈
### 2.1.3 队列
### 2.1.4 dfs
combination sum 1-4 lc 39， 40， 216， 377
lc200
### 2.1.5 bfs
lc46

## 2.2 一直向前！贪心算法
### 2.2.1 硬币问题
这个问有1，5，10，50，100，500 的硬币各数枚，而且一定有解， 问最少用几个硬币组成。
这个问题能用贪心，应该是要求所有面值都可以被整除。
### 2.2.2 区间问题
有meeting 数个，相互可能有交叉， 问至多能参加多少meeting
按结束时间排序， 然后每次贪最早结束的meeting
### 2.2.3 字典序最小问题
给一个字符串s，每次可以取字符串首尾部append到t上， 问怎么组成字典序列最小的t
### 2.2.4 其他例题
1. Saruman's army
直线上有N个点，给一个半径R，问最少几个点安装设备才能辐射所有点。
2. fence Repair
讲一个长为L的棍子切成N块，代价是木板的长度，问怎么切开销最小。


## 2.3 记录结果再利用的“动态规划”
### 2.3.1 01背包问题
首先来说，需要搞清楚什么是01背包。
题目： 有n个重量和价值分别为w_i和v_i的物品， 从这些物品中挑选出总重量不超过W的物品， 求挑选方案中的价值总和的最大值
这个被称为01背包的大概原因就是每个物品皆有选和不选的可能性吧。

首先来看一下题目限制， 这个总是容易被忽略

```
1 <= n <= 100
1 <= w_i,v_i <= 100
1<= W
```

这里面有好几种解法，裸递归，记忆化搜索，正着递归，倒着递归
就写一个自己经常用的
```
dp[i+1][j] = 从前i个物品选出重量不超过j的物品时总价值的最大值
dp[i+1][j] = max(dp[i][j], dp[i][j-w[i]] + v[i]  
```
这样的话，复杂度是O(nW) 此处也就是最多打一个100 * 100 的二维表， 所以还好。

但是如果W的值非常大呢？ 就需要转换一下思路。

```
dp[i+1][j] 代表前i个物品中挑选出总价值和为j的总重量的最小值
```
这样的话复杂度就变成了 $n\dot \sum{v_i}$



### 2.3.2
1. 完全背包问题
2. 最长公共子序列
3. 多重部分和问题
4. 最长上升子序列问题
$n^2$的算法很容易想到，看了一下nlogn的算法。

## 2.4 加工并存储的数据结构
### 2.4.1 树和二叉树
### 2.4.2 优先队列和堆
堆是优先队列的一种高效实现，堆的重要特性是儿子的值一定不小于父亲的值。
堆的一种实现是用数组。 左儿子的编号是i * 2 + 1， 右儿子是 +2
1. Expedition
描述：一辆卡车，有油量P， 卡车每开一单位， 需要消耗一单位的汽油，一共N个加油站，每个加油站的位置存在数组A中， 每个站能加的汽油存在B数组，假设油箱无限大，问卡车是否能到达终点， 如果可以最少需要加多少次油。
第一问是lc134
第二问是优先队列，有了第一问，我们就可以按油量多少进行排序， 假装每经过一个加油站， 都不加油，直至汽油为0， 在考虑从前面挑选最大的油站加油。所以每经过一个油站，都可以把油量的大小push到堆里。 以便之后取。

2. fence repair
前面提过这个题， 可以用堆来解决。

### 2.4.3 二叉树搜索
1. 二叉树的基本插入和删除
插入比较简单
删除的话可以按以下操作
1. 需要删除的节点没有左儿子， 那么把右儿子接上
2. 需要删除的节点的左儿子没有右儿子，那么左儿子接上
3. 如果不满足以上的话，就把左儿子的最大子孙挪到被删除的点上。

### 2.4.4 并查集
1. 食物链
题目大意就是有三类动物A,B,C 他们是循环猎杀的关系。 现在给出条件 1 为同类， 2为猎杀关系，问给一组输入，问有多少假话。 比如前面给出 5和6是同类，那么在遇到5吃6就是错误的。
解法：
1. 分层并查集
2. 分类并查集

## 2.5 它们其实都是图

### 2.5.1 图是什么

### 2.5.2 图的表示
1. 邻接矩阵
2. 临街表

### 2.5.3 图的搜素
图的染色问题， 给一个图，用两种颜色是否能涂完所有的点并且相邻的两个点颜色不能重复。

### 2.5.4 最短路问题
单源最短路径: 这个问题就固定个一个起点， 问到其他所有点的最短路径是多少。

1. Bellman-Form 算法
算法大意就是两个点的最远的最近距离是顶点数v-1， 然后对每一条边进行v-1次更新。
> $d[i] = min{d[j] + (从j到i的边的权值) \| e=(j, i) \\in E}$

所以整个算法的复杂度是VE

2. Dijkstra 算法
3. 任意亮点间的最短路径问题
4. 路径还原

### 2.5.5 最小生成树
1. prim 算法
和Dijkstra很像
2. Kruskal 算法

### 2.5.6 应用问题
1. 次短距离
2. 招兵问题
3.
## 2.6 数学问题的解题窍门
### 2.6.1 碾转相互法
