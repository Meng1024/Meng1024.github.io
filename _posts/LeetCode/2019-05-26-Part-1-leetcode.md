---
layout:     post
title:      "[LeetCode] 小总结"
subtitle:   "leetcode"
categories: LeetCode
date:       2019-05-26
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - LeetCode
---

这个是刷leetcode的小总结， 肯定不可能涵盖所有题目，这只是偶尔觉得想写的时候，写一下。。。

### K distant apart
下面这三道题，给我们一个字符串str或者数组，还有和一个整数k，让我们对其重新排序，使得其中相同的数字或者字符之间的距离不小于k。

 1. 358-Rearrange String k Distance Apart
 2. 621-Task Scheduler
 3. 1054-Distant Barcodes

基本思路就是开一个priority queue，按照出现的次数从大到小排序。再加一个冷冻queue，大小为k
```
class Solution {
    public int[] rearrangeBarcodes(int[] barcodes) {
        if(barcodes == null || barcodes.length == 0) return new int[0];
        Map<Integer, Integer> map = new HashMap<>();
        for(int val : barcodes) {
            map.put(val, map.getOrDefault(val, 0) + 1);   
        }
        PriorityQueue<Map.Entry<Integer, Integer>> pq = new PriorityQueue<>((a, b) -> (b.getValue() - a.getValue()));
        pq.addAll(map.entrySet());
        Queue<Map.Entry<Integer, Integer>> queue = new LinkedList<>();

        int[] ret = new int[barcodes.length];
        int index = 0;
        while(!pq.isEmpty()) {
            Map.Entry<Integer, Integer>  cur = pq.poll();
            ret[index++] = cur.getKey();
            cur.setValue(cur.getValue() - 1);
            queue.add(cur);
            if(queue.size() < 2) {
                continue;
            } else {
                Map.Entry<Integer, Integer> front = queue.poll();
                if(front.getValue() > 0) pq.offer(front);
            }
        }
        return ret;
    }
}
```
