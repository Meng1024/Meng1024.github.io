---
layout:     post
title:      "Priority Queue Summary"
subtitle:   "贪心小结"
categories: Algorithm
date:       2020-03-03
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
hidden: true
tags:
  - Leetcode
  - Algorithm
  - secret

---

## Priority Queue Summary
## 建堆
堆的性质， 以小根堆为例子， 父亲的节点不大于儿子的节点。 这样的数据结构成为堆
### Insert
插入操作：插入在最后面，然后和自己的父亲相交换直到不大于自己的父亲节点。 复杂度 log(n)
### pop
删除操作， 把最后一个元素与第一个元素交换，在向下交换。 复杂度 log(n)
### 建堆
因为是从底向上建的，这样，有1/2的元素向下比较了一次，有1/4的向下比较了两次，1/8的，向下比较了3次，......，1/2^k的向下比较了k次，其中1/2^k <= 1。 所以所有移动的总和为 $\sum_{k=1}^{logn} \Big[\frac{n}{2^{k}} * k\Big]$ 所以最后经过计算就是 O(n) 复杂度

```
import java.util.*;
import java.lang.*;
class Heap{
    int[] heap;
    int sz = 0;
    public Heap(int[] items) {
        this.heap = items;
        this.sz = items.length;
        int i = items.length - 1;
        while(i >= 0) {
            heapify(i);
            i--;
        }
        for(int j : heap) System.out.print(j + " ");
        System.out.println("HEAP builded");
    }
    public void push(int x) {
        int i = sz++;
        while(i > 0) {
            int parent = (i - 1) / 2;
            if(heap[parent] <= x) break;
            heap[i] = heap[parent];
            i = parent;
        }
        heap[i] = x;
    }
    public int pop() {
        if(sz  == 0) return -1;
        int ret = heap[0];
        this.sz--;
        heap[0] = heap[sz];
        heapify(0);
        return ret;
    }

    public void heapify(int x) {
        int i  = x;
        while(i * 2 + 1 < sz) {
            int l = i * 2 + 1, r = i * 2 + 2;
            if(r < sz && heap[r] < heap[l]) l = r;
            if(heap[l] > heap[i]) break;
            int tmp = heap[i];
            heap[i] = heap[l];
            heap[l] = tmp;
            i = l;
        }
    }

    public static void main(String[] args) {
        int[] item = new int[]{5,4,3,2,1,9,6,7,4};
        Heap test = new Heap(item);
        int ret = 0;
        while(ret != -1) {
            ret = test.pop();
            System.out.print(ret + " ");
        }
    }
}
```
## 经典案例
### Argus
编写一个系统， 该系统支持一个 register 命令。来注册不同时间， 且每个 period 时间， 就会才生一个 Q_num 事件。 输出前 k 个时间。  
这个解法就是直接用一个 priorityqueue 每次取出当前事件，之后在 push 一个新的时间到 pq 里面就可以了。

### K个最小和
有 k 个整数数组， 各包含k个元素， 在每一个数组中取出一个元素加起来，可以得到 $k^k$ 个和， 输出最小的和的前 k 项。  
这个题，我们可以从简单的考虑起来。假设只有两个数组长度为 n，求出前 k 项最小和。 最简单的算法就是直接把所有组合 push 到 pq 中，然后取出前 k 项即可。 但是这样做的话， 复杂度是 n^2logn^2. 这里，我们提供两种更好的方法。

**解法1**: 我们各取数组的前 k 个元素， 然后维护一个 k 大小的大端 heap，这样每次 push 的时候，都进行比较。只有比堆顶元素小的时候，我们 pop 堆顶，然后 push 进去。 k^2logk.
```
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> b[0] + b[1] - a[0] - a[1]);
        for(int i = 0; i < nums1.length && i < k; i++) {
            for(int j = 0; j < nums2.length && j < k; j++) {
                if(pq.size() < k) {
                    pq.offer(new int[]{nums1[i], nums2[j]});
                } else if(pq.size() >= k && ((nums1[i] + nums2[j]) < (pq.peek()[0] + pq.peek()[1]))) {
                    pq.poll();
                    pq.offer(new int[]{nums1[i], nums2[j]});
                }
            }
        }
        List<List<Integer>> ret = new ArrayList<>();
        List<Integer> tmp;
        while(!pq.isEmpty()) {
            int[] cur = pq.poll();
            tmp = new ArrayList<>();
            tmp.add(cur[0]);
            tmp.add(cur[1]);
            ret.add(new ArrayList<>(tmp));
        }
        return ret;                                        
    }
}
```

**解法2**：最优解能使算法的复杂度降到 klogk。 我们看以下数列的前两行，我们发现从左到右，从上到下是递增的。 唯有斜对角的关系我们不知道。那么我们可以先把第一列或者第一行的所有和放入 pq 中， 这样我们每取出一个数，在 push 相对应的下一项即可。

$ A_1 + B_1 <= A_1 + B_2 <= A_1 + B_3 ... $  
$ A_2 + B_1 <= A_2 + B_2 <= A_2 + B_3 ... $
.  
.  
.  
$ A_n + B_1 <= A_n + B_2 <= A_n + B_3 ... $

```
class Solution {
    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {
        if(nums2.length == 0 || nums1.length == 0) return new ArrayList<>();
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[0] + a[1] - b[0] - b[1]);
        for(int i = 0; i < nums1.length && i < k; i++) {
            pq.offer(new int[]{nums1[i], nums2[0], 0});
        }
        List<List<Integer>> ret = new ArrayList<>();
        int i = 0;
        List<Integer> tmp;
        while(!pq.isEmpty() && i < k) {
            int[] cur = pq.poll();
            tmp = new ArrayList<>();
            tmp.add(cur[0]);
            tmp.add(cur[1]);
            ret.add(tmp);
            if(cur[2] + 1 < k && cur[2] + 1 < nums2.length) pq.offer(new int[]{cur[0], nums2[cur[2] + 1], cur[2] + 1});
            i++;
        }
        return ret;
    }
}
```

| No. | title                                                                                                        | difficulty      |
|--------------------------------------------------------------------------------------------------------------------|-----------------|
| 42  | [Merge k Sorted Lists]({% post_url 2020-03-08-leetcode-23 %})                                                | Hard            |
| 347 | [Top K Frequent Elements]({% post_url 2020-03-11-leetcode-347 %})                                            | Medium          |
| 218 | [The Skyline Problem]({% post_url 2020-04-10-leetcode-218 %})                                                | Hard            |
| 253 | [Meeting Rooms II]({% post_url 2020-04-16-leetcode-253 %})                                                   | Medium          |
| 215 | [Kth Largest Element in an Array]({% post_url 2020-04-18-leetcode-215 %})                                    | Medium          |
| 973 | [K Closest Points to Origin]({% post_url 2020-04-18-leetcode-973 %})                                         | Medium          |
