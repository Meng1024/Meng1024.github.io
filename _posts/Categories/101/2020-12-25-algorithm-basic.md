---
layout:     post
title:      "Basic Algorithm"
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


# 基础算法
## 读入
Scanner
```
import java.util.*;
import java.io.*;
public static void main(String[] args) {
    Scanner scan = new Scanner(System.in);
    int n = scan.nextInt();

    int[] arr = new int[n];
    for (int i = 0; i < n; i++) {
        arr[i] = scan.nextInt();
    }
    qsort(arr, 0, n - 1);
    for (int i = 0; i < n; i++) {
        System.out.print(arr[i] + (i == n - 1 ? "\n": " "));
    }
}
```
BufferedReader
```
public class Main{
    public static void main(String[] args) throws IOException {
        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
        int n = Integer.parseInt(reader.readLine());
        int[] nums = new int[n];
        String[] strs = reader.readLine().split(" ");
        for (int i = 0; i < n; i++)
            nums[i] = Integer.parseInt(strs[i]);
        // ...
        reader.close(); // 记得关闭
    }
}
```


## 快速排序
### quicksort
随机选择一个数与最左交换
依次递归排左边右边
```
public static void swap(int[] arr, int x, int y) {
    int tmp = arr[x];
    arr[x] = arr[y];
    arr[y] = tmp;
    return;
}
public static void qsort(int[] arr, int l, int r) {
    if(l >= r) return;
    int index = (int)(Math.random() * (r - l + 1)) + l;
    int v = arr[index];
    int st = l - 1, end = r + 1;
    while(st < end) {
        while(arr[++st] < v);
        while(arr[--end] > v);
        if(st < end)swap(arr, st, end);
    }
    qsort(arr, l, end);
    qsort(arr, end + 1, r);
}
```

### find Kth largest number
O(n)
```
public static void swap(int[] arr, int i, int j) {
   int tmp = arr[i];
   arr[i] = arr[j];
   arr[j] = tmp;
}
public static int findKth(int[] arr, int k, int l, int r) {
   if(l >= r) return arr[r];
   int index = (int)(Math.random() * (r - l + 1)) + l;
   int v = arr[index];
   int start = l - 1, end = r + 1;
   while(start < end) {
       while(arr[++start] < v);
       while(arr[--end] > v);
       if(start < end) swap(arr, start, end);
   }

   if(end - l + 1 < k) {
       return findKth(arr, k - (end - l + 1), end + 1, r);
   } else {
       return findKth(arr, k, l, end );
   }
}
```

## Merge sort
### 归并排序
O(nlogn)
```
public static void mergeSort(int[] arr, int l, int r) {
    if(l >= r) return;
    int mid = l + r >> 1;
    mergeSort(arr, l, mid);
    mergeSort(arr, mid + 1, r);
    int k = 0; int i = l, j = mid + 1;
    int[] tmp = new int[r -l + 1];
    while(i <= mid && j <= r) {
        if(arr[i] < arr[j]) {
            tmp[k++] = arr[i++];
        } else {
            tmp[k++] = arr[j++];
        }
    }
    while(i <= mid) tmp[k++] = arr[i++];
    while(j <= r) tmp[k++] = arr[j++];
    for (i = l, j = 0; i <= r; i ++, j ++ ) arr[i] = tmp[j];
    return;
}
```

### 逆序对
O(nlogn)
```
public static long solve(int[] arr, int l, int r) {
    if(l >= r) return 0;
    int mid = l + r >> 1;
    long left = solve(arr, l, mid);
    long right = solve(arr, mid + 1, r);
    int k = 0; int i = l, j = mid + 1;
    int[] tmp = new int[r - l + 1];
    long ret = 0;
    while(i <= mid && j <= r) {
        if(arr[i] <= arr[j]) {
            tmp[k++] = arr[i++];
        } else {
            ret +=  mid - i + 1;
            tmp[k++] = arr[j++];
        }
    }
    while(i <= mid) {
        tmp[k++] = arr[i++];
    }
    while(j <= r) tmp[k++] = arr[j++];
    for (i = l, j = 0; i <= r; i ++, j ++ ) arr[i] = tmp[j];
    return left + right + ret;
}
```

## 二分
### 二分模版
O(logn)
r指向是不满足func的第一个位置。
```
public static int findL(int[] arr, int k) {
    int l = 0, r = arr.length;
    while(l < r) {
        int mid = l + (r - l) / 2;
        if(arr[mid] < k) {
            l = mid + 1;
        } else {
            r = mid;
        }
    }
    return r == arr.length ? -1 : arr[r] == k ? r : -1;
}
```
### 数的三次方根
```
public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    double target = in.nextDouble();
    double l = -10000, r = 10000;
    while (r - l > 1e-8) {      
        double mid = (l + r) / 2;
        if (mid * mid * mid >= target)     
            r = mid;    
        else
            l = mid;
    }
    System.out.println(String.format("%.6f", l));
}
```
## 高精度
这四道题都是要注意有木有前置零。 复杂度为O(n);
### 高精度加法
```
public static String add(String a, String b) {
    StringBuilder sb = new StringBuilder();
    int i = a.length() - 1, j = b.length() - 1;
    int carry = 0;
    while (i >= 0 || j >= 0) {
        int tmp = 0;
        if(i >= 0) tmp += a.charAt(i) - '0';
        if(j >= 0) tmp += b.charAt(j) - '0';
        j--; i--;
        tmp += carry;
        sb.append(tmp % 10 + "");
        carry = tmp / 10;
    }
    if(carry != 0) sb.append(carry + "");
    return sb.reverse().toString();
}
```
### 高精度减法
比较 a 和 b 的大小，如果 a 比 b 小的话， 就算 b - a 最后加上负号
```
class Main {
    public static boolean cmp(String a, String b) {
        if(a.length() != b.length()) return a.length() > b.length();
        int i = 0;
        while(i < a.length()) {
            if(a.charAt(i) == b.charAt(i)) i++;
            else {
                int x = a.charAt(i) - '0', y = b.charAt(i) - '0';
                if(x < y) return false;
                else return true;
            }
        }
        return true;
    }
    public static String sub(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1, j = b.length() - 1;
        int carry = 0;
        while (i >= 0 || j >= 0) {
            int x =  (a.charAt(i) - '0');
            if(j >= 0) x -= (b.charAt(j) - '0') ;
            x += carry;
            sb.append((x + 10) % 10 + "");
            carry = x < 0 ?  -1 : 0;
            j--; i--;
        }  
        //去掉前置0
        String ret = sb.reverse().toString();
        int k = 0;
        while(k < ret.length()) {
            if(ret.charAt(k) != '0') break;
            k++;
        }
        return k == ret.length() ? "0" : ret.substring(k);

    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        String a = scan.next();
        String b = scan.next();
        String ret;
        //if(cmp(a, b)) System.out.println("true");
        if(cmp(a, b)) ret = sub(a, b);
        else ret = "-" + sub(b, a);
        System.out.println(ret);
    }
}
```
### 高精度乘法
这个题是一个大数乘以一个小于100000 的数, 注意处理进位的方式。。
```
public static String mul(String a, int b) {
    StringBuilder sb = new StringBuilder();
    int carry = 0;
    for(int i = a.length() - 1; i >= 0 || carry != 0; i--) {
        int v = 0;
        if(i >= 0) v = (a.charAt(i) - '0') * b;
        v += carry;
        sb.append(v % 10);
        carry = v / 10;
    }
    String ret = sb.reverse().toString();
    //remove leading zeroes;
    int k = 0;
    while(k < ret.length()) {
        if(ret.charAt(k) !=  '0') break;
        k++;
    }
    return k == ret.length() ? "0" : ret.substring(k);
}
```
### 高精度除法
```
public static String div(String a, int b) {
   StringBuilder sb = new StringBuilder();
   int r = 0;
   for(int i = 0; i < a.length(); i++) {
       r += a.charAt(i) - '0';
       sb.append(r / b);
       r %= b;
       r *= 10;
   }
   String ret = sb.toString();
   //remove leading zeroes;
   int k = 0;
   while(k < ret.length()) {
       if(ret.charAt(k) !=  '0') break;
       k++;
   }
   if(k == a.length()) return "0" + "\n" + r / 10;
   return  ret.substring(k) + "\n" + r / 10;
}
```

## 前缀和与差分
### 一维前缀和  
$S_0 = 0, S_i = a_1 + a_2 ... + a_i$  
$sum(L, R) = a_l + a_(l+1) ... + a_r = sum(R) - sum(L - 1)$
```
int[] sum = new int[n + 1];
for(int i  = 1; i < sum.length; i++) {
  sum[i] = sum[i-1] + arr[i-1];
}
```
### 二维前缀和
前缀和数组 这样更新: $s[i][j] = s[i-1][j] + s[i][j-1] - s[i-1][j-1] + a[i][j]$  
左上$(x_1, y_1)$, 右下 $(x_2, y_2)$  矩阵面积就是 $s[x_2, y_2] - s[x_1 -1, y_2] - s[x_2, y_1 - 1] + s[x_1 - 1, x_2 - 1]$

```
class Main {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), m = scan.nextInt(), k = scan.nextInt();
        int[][] s = new int[n + 1][m + 1];

        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                s[i][j] = scan.nextInt();
            }
        }
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                s[i][j] = s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1] + s[i][j];
            }
        }
        for(int i = 0;  i < k; i++) {
            int x1 = scan.nextInt(), y1 = scan.nextInt(), x2 = scan.nextInt(), y2 = scan.nextInt();
            System.out.println(s[x2][y2] - s[x1 - 1][y2] - s[x2][y1 - 1] + s[x1 - 1][y1 - 1]);
        }
    }
}
```
### 差分数组
给定a[1], a[2], ... , a[n]  
构造差分数组使得差分数组是原数组的前缀和数组 a[i] = b[1] + b[2] + ... + b[i]

核心操作:  
将a[L~R]全部加上c 等价于 b[L] += c, b[R + 1] -= c

```
AC797
class Main {
    public static void insert(int[] arr, int l, int r, int c) {
        arr[l] += c;
        arr[r + 1] -= c;
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), k = scan.nextInt();
        int[] arr = new int[100010];
        for(int i = 0; i < n; i++) {
            int v = scan.nextInt();
            insert(arr, i, i, v);
        }
        while(k > 0) {
            int l = scan.nextInt(), r = scan.nextInt(), v= scan.nextInt();
            insert(arr, l - 1, r - 1, v);
            k--;
        }

        int tmp = 0;
        for(int i = 0; i < n; i++) {
            tmp += arr[i];
            System.out.print(tmp + " ");
        }
    }
}
```

### 二维差分矩阵
给定原矩阵a[i, j], 构造差分矩阵使得a是s的二维前缀和。
核心操作： 给以 $(x_1, y_1)$ 为左上角，$(x_2, y_2)$ 为右下角的子矩阵中所有数a[i,j]加上c
等价于在差分矩阵 做如下修改
 ```
 s[x1,y1] += c
 s[x2 + 1,y1] -= c
 s[x1, y2 + 1] -= c
 s[x2 + 1, y2 + 1] += c
```

```
class Main {
    public static void insert(int[][] s, int x1, int y1, int x2, int y2, int c) {
         s[x1][y1] += c;
         s[x2 + 1][y1] -= c;
         s[x1][y2 + 1] -= c;
         s[x2 + 1][y2 + 1] += c;
    }
    public static void main(String[] args) throws IOException {
        Scanner scan = new Scanner(System.in);
        OutputStreamWriter writer = new OutputStreamWriter(System.out);
        int n = scan.nextInt(), m = scan.nextInt(), k = scan.nextInt();
        int[][] s = new int[n + 2][m + 2];
        for(int i = 1; i <= n ; i++) {
            for(int j = 1; j <= m; j++) {
                int v = scan.nextInt();
                insert(s, i, j, i, j, v);
            }
        }

        while(k > 0) {
            int x1 = scan.nextInt(), y1 = scan.nextInt();
            int x2 = scan.nextInt(), y2 = scan.nextInt();
            int c = scan.nextInt();
            insert(s, x1, y1, x2, y2, c);
            k--;
        }
        int tmp = 0;
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= m; j++) {
                s[i][j] += s[i - 1][j] + s[i][j - 1] - s[i - 1][j - 1];
                writer.write(s[i][j] + " ");
            }
            writer.write("\n");
        }
        writer.flush();
        scan.close();
        writer.close();
    }
}
```
## 双指针
### 最长连续不重复子序列
滑动窗口经典例题
```
int[] window = new int[100010];
int ret = 1;
for(int i = 0, j = 0; j < n; j++) {
    window[arr[j]]++;
    while( i < j && window[arr[j]] > 1) {
        window[arr[i]]--;
        i++;
    }
    ret = Math.max(ret, j - i + 1);
}
```

###  数组元素的目标和
给定两个排好序的且没有重复元素的数组，找出从两个数组选出两个元素和为 k 的下标
```
public static int[] solve(int[] s, int[] t, int k) {
   int j = 0, ret = 0;
   for(int i = 0; i < s.length; i++) {
       j = t.length - 1;
       while(j >= 0 && s[i] + t[j] > k) {
           j--;
       }
       if(s[i] + t[j] == k) return new int[]{i, j};
   }
   return new int[0];
}
```

### 判断子序列
```
public static boolean isSeq(int[] a, int[] b) {
    if(a.length > b.length) return false;
    int i = 0, j = 0;
    while(i < a.length && j < b.length) {
        if(a[i] == b[j]) {
            i++; j++;
        } else {
            j++;
        }
    }
    return i == a.length;
}
```

## 位运算
常用位运算操作：
1. 求n的第k位数字 n >> k & 1
2. 返回n的最后一位1 lowbit(n) = n & -n;

### 二进制中1的个数
```
public static int getBit(int n) {
    int ret = 0;
    while(n != 0) {
        n -= n & -n;
        ret++;
    }
    return ret;
}
```

## 离散化
主要是映射。。。
```
import java.util.*;
import java.io.*;
class Main {
    public static int getIndex(List<Integer> list, int v) {
        int l = 0, r = list.size();
        while(l < r) {
            int mid = l + (r - l) / 2;
            if(list.get(mid) <= v) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return r == 0 ? 0 : r-1;
    }
    public static List<Integer> helper(List<Integer> list) {
        List<Integer> ret = new ArrayList<>();
        ret.add(list.get(0));
        for(int i : list) {
            ret.add(i + 1);
        }
        return ret;
    }
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int n = scan.nextInt(), m = scan.nextInt();

        TreeMap<Integer, Integer> tree = new TreeMap<>();
        for(int i = 0; i < n; i++) {
            int k = scan.nextInt(), v = scan.nextInt();
            tree.putIfAbsent(k, 0);
            tree.put(k, tree.get(k) + v);
        }
        List<Integer> list = new ArrayList(tree.keySet());

        int[] sum = new int[list.size() + 1];
        for(int i = 1; i < sum.length; i++) {
            sum[i] = sum[i - 1] + tree.get(list.get(i - 1));
            //System.out.print(sum[i] + " ");
        }
        list = helper(list);

        while(m > 0) {
            int l = scan.nextInt(), r = scan.nextInt();
            l = getIndex(list, l);
            r = getIndex(list, r+1);
            //System.out.println(list.get(l) + " " + list.get(r));
            System.out.print(sum[r] - sum[l] + "\n");
            m--;
        }
    }
}
```

## 区间合并
```
Collections.sort(list, (a, b) -> (a[0] - b[0]));
int[] cur = list.get(0);
int ret = 1;
for(int i = 1; i < list.size(); i++) {
    int[] next = list.get(i);
    if(cur[1] < next[0]) {
        ret++;
        cur = next;
    } else {
        cur[1] = Math.max(cur[1], next[1]);
    }
}
```
