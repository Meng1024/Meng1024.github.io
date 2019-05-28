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

### My Calendar I, II, III
1. 729-My Calendar I
2. 731-My Calendar II
3. 732-My Calendar III

Calendar I是问是否能够成功book一个meeting without conflicting. 用treemap 存开始和结束位置，每次有一个新的book的话，就检查是否合格, 因为treemap里面存的不可能有conflicts 的情况，所以直接找floorkey 和ceilkey就可以。
```
class MyCalendar {
    TreeMap<Integer, Integer> calendar;

    MyCalendar() {
        calendar = new TreeMap();
    }
    public boolean book(int start, int end) {
        Integer prev = calendar.floorKey(start),
                next = calendar.ceilingKey(start);
        if ((prev == null || calendar.get(prev) <= start) &&
                (next == null || end <= next)) {
            calendar.put(start, end);
            return true;
        }
        return false;
    }
}
```
Calendar II 和Calendar III 归到根本就是问，在同一个时间段最大的meeting 数。
这样的话，我门遇到起始位置就+1，遇到结束位置就-1. 然后设置一个变量记录峰值即可。这波骚操作解决一个hard。。。 哈哈哈
```
class MyCalendarThree {
    TreeMap<Integer, Integer> map;
    public MyCalendarThree() {
        map = new TreeMap<>();
    }

    public int book(int start, int end) {
        map.put(start, map.getOrDefault(start, 0) + 1);
        map.put(end, map.getOrDefault(end, 0) - 1);
        int ret = 0;
        int tmp = 0;
        for(int d : map.values()) {
            tmp += d;
            ret = Math.max(ret, tmp);
        }
        return ret;
    }
}
```
