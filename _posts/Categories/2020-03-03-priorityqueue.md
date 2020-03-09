---
layout:     post
title:      "Priority Queue Summary"
subtitle:   "贪心小结"
categories: Algorithm
date:       2020-03-03
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
  - Leetcode
  - Algorithm
  - secret

---

## Priority Queue Summary
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
