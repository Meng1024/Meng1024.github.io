---
layout:     post
title:      "水塘抽样-Reservoir Sampling"
subtitle:   "leetcode"
categories: LeetCode
date:       2019-06-05
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
hidden: true
tags:
  - LeetCode
---

### 水塘抽样
解决一个蛋疼的问题。。 就是可否在一未知大小的集合中，随机取出一元素使其概率为1/N

解法就是第一次直接以第一个数作为ret，而后第二次以二分之一概率决定是否用第二行替换ret ，第三次以三分之一的概率决定是否以第三行替换ret ……，以此类推。在取第i个数据的时候，我们生成一个0到1的随机数p，如果p小于1/i, 替换当前的ret为当前的值。如果大于1/i，ret不变。直到数据流结束，返回此数，算法结束

分析一下取出i的概率， 对于i之前ret如何都无所谓，假设i是5，

p_i = 1/5 * 5/6 * 6/7 ... * n-1/n

推出来p_i 的概率是 1/n

由此可解 382. Linked List Random Node

```
class Solution {
    ListNode cur;
    public Solution(ListNode head) {
        this.cur = head;
        System.out.println(cur.val);
    }

    /** Returns a random node's value. */
    public int getRandom() {
        ListNode head = cur;
        int ret = cur.val;
        int i = 2;
        cur = cur.next;
        while(cur != null) {
            if(Math.random() * i < 1) {
                ret = cur.val;
            }
            i++;
            cur = cur.next;
        }
        cur = head;
        return ret;
    }
}
```

这个问题的扩展就是：如何从未知或者很大样本空间随机地取k个数？亦即是说，如果档案共有N ≥ k行，则每一行被抽取的概率为k/N。

根据上面（随机取出一元素）的分析，我们可以把上面的1/n变为k/n即可。思路为：在取第n个数据的时候，我们生成一个0到1的随机数p，如果p小于k/n，替换池中任意一个为第n个数。大于k/n，继续保留前面的数。直到数据流结束，返回此k个数。但是为了保证计算机计算分数额准确性，一般是生成一个0到n的随机数，跟k相比，道理是一样的。
