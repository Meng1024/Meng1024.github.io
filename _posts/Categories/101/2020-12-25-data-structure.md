---
layout:     post
title:      "Data Structure"
subtitle:   "贪心小结"
categories: Algorithm
date:       2020-12-25
author:     "Meng"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
hidden: true
tags:
  - Algorithm
  - secret

---




# 数据结构
## 单链表
```
class LinkList {
    int N = 100010;
    // head 表示头结点的下标
    // e[i] 表示节点i的值
    // ne[i] 表示节点i的next指针是多少
    // idx 存储当前已经用到了哪个点
    int head, idx;
    int[] e, ne;
    public LinkList() {
        head = -1;
        idx = 0;
        e = new int[N];
        ne = new int[N];
        //Arrays.fill(ne, -1);
    }
    public void insertH(int v) {
        e[idx] = v;
        ne[idx] = head;
        head = idx++;
    }
    public void deleteK(int k) {
        if(k == -1) {
            head = ne[head];
            return;
        }
        ne[k] = ne[ne[k]];
    }
    public void insertK(int k, int v) {
        if(k == -1) {
            insertH(v);
            return;
        }
        e[idx] = v;
        ne[idx] = ne[k];
        ne[k] = idx++;
    }
    public void print() {
        for (int i = head; i != -1; i = ne[i]) {
            System.out.print(e[i] + " ");
        }
    }
}
```
## 双链表
```
class LinkList {
    int N = 100010;
    int idx;
    int[] e, l, r;
    public LinkList() {
        l = new int[N];
        r = new int[N];
        e = new int[N];
        r[0] = 1;
        l[1] = 0;
        idx = 2;
    }
    public void insert(int a, int x) {
        e[idx] = x;
        l[idx] = a;
        r[idx] = r[a];
        l[r[a]] = idx;
        r[a] = idx++;
    }
    public void remove(int a) {
        l[r[a]] = l[a];
        r[l[a]] = r[a];
    }
    public void print() {
        for(int i = r[0]; i != 1; i = r[i]) {
            System.out.print(e[i] + " ");
        }
    }
}
```

## 栈
```
class Main {
    public static void main(String[] args) {
        int n = 100010;
        int[] stack = new int[n];
        int idx = -1;
        Scanner scan = new Scanner(System.in);
        int m = scan.nextInt();
        while(m > 0) {
            String op = scan.next();
            int v;
            switch(op) {
                case "push":
                    v = scan.nextInt();
                    stack[++idx] = v;
                    break;
                case "pop":
                    idx--;
                    break;
                case "empty":
                    if(idx == -1) System.out.println("YES");
                    else System.out.println("NO");
                    break;
                case "query":
                    System.out.println(stack[idx]);
            }
            m--;
        }
    }
}
```

## 队列
```
class Main {
    public static void main(String[] args) {
        int n = 100010;
        int[] queue = new int[n];
        int h = -1, t = -1;
        Scanner scan = new Scanner(System.in);
        int m = scan.nextInt();
        while(m > 0) {
            String op = scan.next();
            switch(op) {
                case "push" :
                    int k = scan.nextInt();
                    queue[++t] = k;
                    break;
                case "empty":
                    if(h == t) System.out.println("YES");
                    else System.out.println("NO");
                    break;
                case "pop":
                    h++;
                    break;
                case "query" :
                    System.out.println(queue[h + 1]);
            }
            m--;
        }
    }
}
```

## 单调栈
```
for(int i = 0; i < n; i++) {
    while(stack.size() != 1 && arr[stack.peek()] >= arr[i]) stack.pop();
    if(stack.size() == 1) System.out.print("-1 ");
    else System.out.print(arr[stack.peek()] + " ");
    stack.push(i);
}
```

## 滑动窗口
输出选定区间大小为k的最大值和最小值。
```
public static void solve(int[] arr, int k) {
    Deque<Integer> queue = new LinkedList<>();

    for(int i = 0; i < arr.length; i++) {
        while(!queue.isEmpty() && queue.peekFirst() < i - k + 1) queue.pollFirst();
        while(!queue.isEmpty() && arr[queue.peekLast()] >= arr[i]) queue.pollLast();
        queue.offer(i);
        if(i >= k - 1) System.out.print(arr[queue.peekFirst()] + " ");
    }
    System.out.println();
    queue.clear();
    for(int i = 0; i < arr.length; i++) {
        while(!queue.isEmpty() && queue.peekFirst() < i - k + 1) queue.pollFirst();
        while(!queue.isEmpty() && arr[queue.peekLast()] <= arr[i]) queue.pollLast();
        queue.offer(i);
        if(i >= k - 1) System.out.print(arr[queue.peekFirst()] + " ");
    }
}
```

## KMP
对模版串预处理后缀和前缀最大是多少。
```
public static void solve(char[] s, char[] p) {
    int[] ne = new int[100010];
    ne[0] = -1;
    for (int i = 1, j = -1; i < p.length; i ++ ) {
        while (j >= 0 && p[j + 1] != p[i]) j = ne[j];
        if (p[j + 1] == p[i]) j ++ ;
        ne[i] = j;
    }

    for (int i = 0, j = -1; i < s.length; i ++ ) {
        while (j != -1 && s[i] != p[j + 1]) j = ne[j];
        if (s[i] == p[j + 1]) j ++ ;
        if (j == p.length - 1) {
            System.out.println(i - j + " ");
            j = ne[j];
        }
    }
}
```

## Trie
### Trie 字符串统计
```
class Trie {
    Trie[] trie;
    int cnt = 0;
    public Trie() {
        this.trie = new Trie[26];
    }
    public void insert(String s) {
        Trie root = this;
        for(int i = 0; i < s.length(); i++) {
            int idx = s.charAt(i) - 'a';
            if(root.trie[idx] != null) root = root.trie[idx];
            else {
                root.trie[idx] = new Trie();
                root = root.trie[idx];
            }
        }
        root.cnt++;
    }
    public int query(String s) {
        Trie root = this;
        for(int i = 0; i < s.length(); i++) {
            int idx = s.charAt(i) - 'a';
            if(root.trie[idx] != null) root = root.trie[idx];
            else {
                return 0;
            }
        }
        return root.cnt;
    }
}
```

### 最大异或对
```
class Trie {
    Trie[] trie;
    int cnt = 0;
    public Trie() {
        this.trie = new Trie[2];
    }
    public void insert(int v) {
        Trie root = this;
        for(int i = 30; i >= 0; i--) {
            int idx = (v >> i & 1);
            if(root.trie[idx] == null) root.trie[idx] = new Trie();
            root = root.trie[idx];
        }
    }
    public int search(int v) {
        Trie root = this;
        int ret = 0;
        for(int i = 30; i >= 0; i--) {
            int idx = (v >> i & 1 ) == 1 ? 0 : 1;
            if(root.trie[idx] != null) {
                root = root.trie[idx];
                ret += (1 << i);
            }
            else if (root.trie[1 - idx] != null) {
                root = root.trie[1 - idx];
            } else {
                return 0;
            }
        }
        return ret;
    }
}
```

## 并查集
### 合并集合
```
public static void union(int[] uf, int x, int y) {
     int rx = find(uf, x), ry = find(uf, y);
     if(rx == ry) return;
     uf[rx] = ry;
 }
 public static int find(int[] uf, int x) {
     if(x == uf[x]) return x;
     uf[x] = find(uf, uf[x]);
     return uf[x];
 }
```

### 连通块中点的数量
```
class Main {
    public static void union(int[] uf, int[] cnt, int x, int y) {
        int rx = find(uf, x), ry = find(uf, y);
        if(rx == ry) return;
        cnt[ry] += cnt[rx];
        uf[rx] = ry;
    }
    public static int find(int[] uf, int x) {
        if(x == uf[x]) return x;
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), m = scan.nextInt();
        int[] uf = new int[n + 1];
        int[] cnt = new int[n + 1];
        Arrays.fill(cnt, 1);
        for(int i = 0; i < uf.length; i++) uf[i] = i;
        while(m > 0) {
            String op = scan.next();
            if(op.equals("Q2")) {
                int v = scan.nextInt();
                int r = find(uf, v);
                System.out.println(cnt[r]);
            } else {
                int x = scan.nextInt(), y = scan.nextInt();
                if(op.equals("C")) union(uf, cnt, x, y);
                if(op.equals("Q1")) System.out.println(find(uf, x) == find(uf, y) ? "Yes" : "No");
            }
            m--;
        }
    }
}
```

### 食物链
```
class Main {
    public static int find(int[] uf, int x, int[] d) {
        if(uf[x] == x) return x;
        //这里不能直接更新uf[x]，否则距离就不是父亲节点到root节点的距离了。
        int t = find(uf, uf[x], d);
        d[x] += d[uf[x]];
        uf[x] = t;
        return uf[x];
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), m = scan.nextInt();
        int[] uf = new int[n + 1];
        for(int i = 0; i < uf.length; i++) uf[i] = i;
        int[] d = new int[n + 1];
        int ret = 0;
        while(m > 0) {
            int l = scan.nextInt(), x = scan.nextInt(), y = scan.nextInt();
            if(x > n || y > n) {
                ret++;
            } else {
                int px = find(uf, x, d);
                int py = find(uf, y, d);
                if(l == 1) {
                    if(px == py) {
                        if ((d[x] - d[y]) % 3 != 0) ret++;;
                    } else {
                        uf[px] = py;
                        // px ---? -- py
                        // | d[x]     | d[y]
                        // x          y
                        // (d[x] + ? - d[y]) % 3 = 0; => ? = d[y] - d[x];  
                        d[px] = d[y] - d[x];
                    }
                }
                if(l == 2) {
                    if(px == py) {
                        if((d[x] - d[y] - 1) % 3 != 0) ret++;
                    } else {
                        // 同理， dx + ? -dy = 1
                        uf[px] = py;
                        d[px] = d[y] + 1 - d[x];
                    }
                }
            }
            m--;
        }
        System.out.print(ret);
    }
}
```

## 堆
stl 支持前3个操作。
1. 插入一个数: heap[++szie], up(size)
2. 求最小值: heap[1]
3. 删除最小值: heap[1] = heap[size--], down(1)
4. 删除任意一个值: heap[k] = heap[size--], down(k), up(k);
5. 修改任意一个值: heap[k] = x; down(k), up(k)

堆：满二叉树，除了最后一行都是满的。且最后一行从左向右依次排布的。  
小根堆性质： 每一个节点都是小于孩子节点的。

堆的存储：一维数组存储。 1号点为根节点， 那么左儿子的坐标 2x， 右儿子 2x + 1
堆的两个操作：
1. down(x) : 往下调整
2. up(x)： 往上调整

建堆：
1. 一个一个插入元素: O(nlogn)
2. 从 n/2 开始往下down: O(n)  
因为是从底向上建的，这样，有1/2的元素向下比较了一次，有1/4的向下比较了两次，1/8的，向下比较了3次 ,..., 1/2^k的向下比较了k次，其中1/2^k <= 1。 所以所有移动的总和为 $\sum_{k=1}^{logn} \Big[\frac{n}{2^{k}} * k\Big]$ 所以最后经过计算就是 O(n)

### 堆排序
```
class Heap {
    int size;
    int[] heap;
    public Heap(int[] h) {
        size = h.length - 1;
        this.heap = h;
        build();
    }
    //建堆
    public void build() {
        for (int i = size / 2; i > 0; i -- ) down(i);
    }
    public void down(int i) {
        int idx = i;
        if(2 * i <= size && heap[i] > heap[2 * i]) idx = 2 * i;
        if(2 * i + 1 <= size && heap[2 * i + 1] < heap[idx]) idx = 2 * i + 1;
        if(i != idx) {
            int tmp = heap[i];
            heap[i] = heap[idx];
            heap[idx] = tmp;
            down(idx);
        }
    }
    public int getMin() {
        int ret = heap[1];
        heap[1] = heap[this.size--];
        down(1);
        return ret;
    }
}
```

### 模拟堆
```
class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt();

        Heap h = new Heap(n);
        int idx = 0;
        while(n > 0) {
            int k, x;
            String op = scan.next();
            switch(op) {
                case "I":
                    x = scan.nextInt();
                    h.size++;
                    idx++;
                    h.heap[h.size] = x;
                    h.ph[idx] = h.size;
                    h.hp[h.size] = idx;
                    h.up(h.size);
                    break;
                case "PM" :
                    System.out.println(h.heap[1]);
                    break;
                case "DM":
                    h.heap_swap(1, h.size);
                    h.size--;
                    h.down(1);
                    break;
                case "D" :
                    k = scan.nextInt();
                    k = h.ph[k];
                    h.heap_swap(k, h.size);
                    h.size--;
                    h.down(k);
                    h.up(k);
                    break;
                case "C":
                    k = scan.nextInt(); x = scan.nextInt();
                    k = h.ph[k]; // 必须提前记录下ph[k]的值，不然ph[k]的值可能会变
                    h.heap[k] = x;  
                    h.down(k);
                    h.up(k);
                    break;
            }
            n--;
        }
    }
}

class Heap {
    int size;
    int[] heap;
    //ph[k] 存的第k个点在堆里面的下标
    //hp[k] 存的是当前点在ph的下表
    int[] hp, ph;
    public Heap(int n) {
        this.size = 0;
        heap = new int[n + 1];
        hp = new int[n + 1];
        ph = new int[n + 1];
    }
    public  void swap(int[] arr, int x, int y) {
        int tmp = arr[x];
        arr[x] = arr[y];
        arr[y] = tmp;
    }
    public void heap_swap(int x, int y) {
        swap(ph, hp[x], hp[y]);
        swap(hp, x, y);  
        swap(heap, x, y);
    }
    public void down(int i) {
        int idx = i;
        if(2 * i <= size && heap[i] > heap[2 * i]) idx = 2 * i;
        if(2 * i + 1 <= size && heap[2 * i + 1] < heap[idx]) idx = 2 * i + 1;
        if(i != idx) {
            heap_swap(i, idx);
            down(idx);
        }
    }
    public void up(int i) {
        while(i / 2 != 0 && heap[i / 2] > heap[i]) {
            heap_swap(i / 2, i);
            i = i / 2;
        }
    }
}
```

## Hash
复杂度 O(1)  
注意:
1. 模的数取质数，这样可以保证冲突的数字尽可能小
2. 删除一般使用标记，不会真正的删除

### 开放寻址法：

只有一个数组，但是长度一般是输入数据的2～3倍
插入的话直接用 h.hash[h.find(x)] = x;
查找的话，直接看h.hash[h.find(x)] 是不是 0x3f3f3f。
模版：
```
//开放寻值法
class Hash {
    int n = 200003;
    int INF = 0x3f3f3f;
    int[] hash = new int[n];
    public Hash() {
        Arrays.fill(hash, INF);
    }
    // 如果存在，返回index 否则返回应该插入的index
    public int find(int x) {
        int index =  (x % n + n) % n;
        while(hash[index] != INF && hash[index] != x) {
            index++;
            if(index == hash.length) index = 0;
        }
        return index;
    }
}
```

### 拉链法 ： 数组 + 单链表
模版：
```
//拉链法
class Hash {
    int n = 100003;
    List<Integer>[] hash = new List[100003];

    public void insert(int x) {
        int index =  (x % n + n) % n;
        if(hash[index] == null) hash[index] = new ArrayList<>();
        hash[index].add(x);
    }
    public boolean find(int x) {
        int index =  (x % n + n) % n;
        if(hash[index] == null) return false;
        for(int i : hash[index]) {
            if(i == x) return true;
        }
        return false;
    }
}
```
### 字符串 hash
字符串前缀哈希法
注意：
- 不能映射成 0,  容易冲突. 否则， A 与 AA hash值一样。
- 如果P = 131 或者 13331， Q = 2 ^ 64 可以假设不存在冲突. 并且可以用 unsigned long long 来存储。不需要取模，溢出不需要处理。

```
e.g str =  "ABCAB"
h[0] = 0;
h[1] = "A" 的hash值
h[2] = "AB" 的hash值
h[3] = "ABC" 的hash值
h[4] = "ABCA" 的hash值

如何定义hash值：P进制法  
eg. ABCD -> (1,2,3,4)P = 1 * P^3 + 2 * P^2 + 3 * P^1 + 4 * P^0
最后模上Q 把它们映射到一个相对小的范围

(R)----(L-1)-(L)------(R)-----|
已知 h[R], h[L-1] 那么R 到 L的 hash 就是 h[R] - h[L] * P ^(R - L + 1)
```

模版：

```
class Main {
    static int N = 100010;
    static long[] p = new long[N];
    static long[] h = new long[N];
    static int P = 131;
    static long mod = Long.MAX_VALUE;
    static void init(char[] str) {
        p[0] = 1;
        for (int i = 1; i <=  str.length; i ++ ) {
            h[i] = (h[i - 1] * P + str[i - 1]) % mod;
            p[i] = p[i - 1] * P;
        }
    }

    static long query(int l, int r) {
        return h[r] - h[l-1] * p[r - l + 1];
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), m = scan.nextInt();
        char[] str = scan.next().toCharArray();
        init(str);
        while(m > 0) {
            int l1 = scan.nextInt(), r1 = scan.nextInt();
            int l2 = scan.nextInt(), r2 = scan.nextInt();
            if(query(l1, r1) == query(l2, r2)) System.out.println("Yes");
            else System.out.println("No");
            m--;
        }
    }
}
```
