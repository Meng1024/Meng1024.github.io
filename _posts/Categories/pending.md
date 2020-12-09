pending:
https://leetcode-cn.com/problems/remove-duplicate-letters/solution/yi-zhao-chi-bian-li-kou-si-dao-ti-ma-ma-zai-ye-b-4/

295. Find Median from Data Stream -- heap
378. Kth Smallest Element in a Sorted Matrix -- heap/binary search
264. Ugly Number II -- heap

Topological sort 建图+ bfs
269. Alien Dictionary -- Topological
444. Sequence Reconstruction
1203. Sort Items by Groups Respecting Dependencies
329. Longest Increasing Path in a Matrix



graph
书上例题
133. Clone Graph
959. Regions Cut By Slashes
138. Copy List with Random Pointer
200. Number of Islands

695. Max Area of Island / 733. Flood Fill
```
class Solution {
    public int dfs(int[][] grid, int x, int y) {
        if(x < 0 || x >= grid.length || y < 0 || y >= grid[x].length || grid[x][y] == 0) return 0;
        int[] dx = {1, -1, 0, 0};
        int[] dy = {0, 0, 1, -1};
        int ret = 0;
        grid[x][y] = 0;
        for(int i = 0; i < dx.length; i++) {
            ret += dfs(grid, x + dx[i], y + dy[i]);
        }
        ret += 1;
        return ret;
    }
    public int maxAreaOfIsland(int[][] grid) {
        if(grid == null || grid.length == 0) return 0;
        int ret = 0;
        for(int i = 0; i < grid.length; i++) {
            for(int j = 0; j < grid[i].length; j++) {
                if(grid[i][j] == 1) {
                    ret = Math.max(ret, dfs(grid, i, j));
                }
            }
        }
        return ret;
    }
}
```
399. Evaluate Division
class Solution {
    public void union(Map<String, String> map, Map<String, Double> ratio, String x, String y, double value) {
        if (!map.containsKey(x)) {
             map.put(x, x);
             ratio.put(x, 1.0);
        }
        if (!map.containsKey(y)) {
             map.put(y, y);
             ratio.put(y, 1.0);
        }
        //ratio.put(y, value);
        String p1 = find(map, ratio, x);
        String p2 = find(map, ratio, y);
        if(p1.equals(p2)) return;
        map.put(p2, p1);
        ratio.put(p2, ratio.get(x) * value / ratio.get(y) );
    }
    public String find(Map<String, String> map, Map<String, Double> ratio, String x) {
        if(map.get(x).equals(x)) return x;

        ratio.put(x, ratio.getOrDefault(map.get(x), 1.0) * ratio.getOrDefault(x, 1.0));
        map.put(x, find(map, ratio, map.get(x)));
        return map.get(x);
    }
    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        // node, parent node
        Map<String, String> map = new HashMap<>();
        Map<String, Double> ratio = new HashMap<>();

        for(int i = 0; i < equations.size(); i++) {
            //ratio(equations.get(i).get(1), values[i]);
            union(map, ratio, equations.get(i).get(0), equations.get(i).get(1), values[i]);
        }
        //for(String key : ratio.keySet()) System.out.println(key);
        double[] ret = new double[queries.size()];
        for(int i = 0; i < queries.size(); i++) {
            String x = queries.get(i).get(0);
            String y = queries.get(i).get(1);

            String s1 = queries.get(i).get(0), s2 =  queries.get(i).get(1);
            if (!map.containsKey(s1) || !map.containsKey(s2)
                || !find(map, ratio, s1).equals(find(map, ratio, s2))) {
                ret[i] = -1.0;
            } else {
                ret[i] = ratio.get(s2) / ratio.get(s1);
            }

        }
        return ret;
    }
}

graph 3

bfs 3
841. Keys and Rooms

dfs 3
130. Surrounded Regions
765. Couples Holding Hands
```
class Solution {
    public void union(int[] uf, int x, int y) {
        int px = find(uf, x);
        int py = find(uf, y);
        if(px == py) return;
        uf[py] = px;
    }
    public int find(int[] uf, int x) {
        if(uf[x] == x) return x;
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }
    public int minSwapsCouples(int[] row) {
        if(row == null || row.length <= 2) return 0;
        int[] uf = new int[row.length/2];
        for(int i = 0; i < uf.length; i++) {
            uf[i] = i;
        }
        for(int i = 0; i < row.length/2; i++) {
            union(uf,  row[2 * i] / 2, row[2 * i + 1] / 2);
        }
        Set<Integer> ret = new HashSet<>();
        for(int i : uf) ret.add(find(uf, i));
        return  row.length/2 - ret.size();
    }
}
```

684. Redundant Connection
```
class Solution {
    public void union(int[] t, int i, int j) {
        int a = find(t, i);
        int b = find(t, j);
        if(a == b) return;
        t[a] = b;
        return;
    }
    public int find(int[] t, int i) {
        if(t[i] == i) return i;
        t[i] = find(t, t[i]);
        return t[i];
    }
    public int[] findRedundantConnection(int[][] edges) {
        int[] t = new int[edges.length + 1];
        for(int i = 0; i < t.length; i++) t[i] = i;
        for(int i = 0; i < edges.length; i++) {
            if(find(t, edges[i][0]) == find(t, edges[i][1])){
                return edges[i];
            } else {
                union(t, edges[i][0], edges[i][1]);
            }
        }
        return new int[2];
    }
}
```


684. Redundant Connection
```
class Solution {
    public void union(int[] t, int i, int j) {
        int a = find(t, i);
        int b = find(t, j);
        if(a == b) return;
        t[a] = b;
        return;
    }
    public int find(int[] t, int i) {
        if(t[i] == i) return i;
        t[i] = find(t, t[i]);
        return t[i];
    }
    public int[] findRedundantConnection(int[][] edges) {
        int[] t = new int[edges.length + 1];
        for(int i = 0; i < t.length; i++) t[i] = i;
        for(int i = 0; i < edges.length; i++) {
            if(find(t, edges[i][0]) == find(t, edges[i][1])){
                return edges[i];
            } else {
                union(t, edges[i][0], edges[i][1]);
            }
        }
        return new int[2];
    }
}
```
tree 3

union find 3
827. Making A Large Island
注意， count的初始化，和合并的时候 用考虑所有neibors
1202. Smallest String With Swaps 敲一下 代码
990. Satisfiability of Equality Equations
399. Evaluate Division

737. Sentence Similarity II
839. Similar String Groups


952. Largest Component Size by Common Factor

超时；
```
class Solution {
    public void union(int[] uf, int[] count, int x, int y) {
        int px = find(uf, x), py = find(uf, y);
        if(px == py) return;
        uf[py] = px;
        count[px] = count[py] + count[px];
    }
    public int find(int[] uf, int x) {
        if(x == uf[x]) return x;
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }

    public int gcd(int a, int b) {
        if (b == 0)
            return a;
        return gcd(b, a % b);  

    }

    public boolean hasCommon(int a, int b) {
        return gcd(a, b) != 1;
    }
    public int largestComponentSize(int[] A) {
        if(A == null || A.length == 0) return 0;
        if(A.length == 1) return 1;
        int[] uf = new int[A.length];
        int[] count = new int[A.length];
        Arrays.fill(count, 1);
        for(int i = 0; i < uf.length; i++) uf[i] = i;
        for(int i = 0; i < A.length; i++) {
            for(int j = i + 1; j < A.length; j++) {
                if(hasCommon(A[i], A[j])) {
                    union(uf, count, i, j);
                }
            }
        }
        int ret = 0;
        for(int i = 0; i < uf.length; i++) {
            ret = Math.max(ret, count[i]);
        }
        return ret;
    }
}
```

```
class Solution {
    public void union(int[] uf, int[] count, int x, int y) {
        int px = find(uf, x), py = find(uf, y);
        if(px == py) return;
        uf[py] = px;
        count[px] = count[py] + count[px];
    }
    public int find(int[] uf, int x) {
        if(x == uf[x]) return x;
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }

    public int largestComponentSize(int[] A) {
        if(A == null || A.length == 0) return 0;
        if(A.length == 1) return 1;
        int[] uf = new int[A.length];
        for(int i = 0; i < uf.length; i++) {
            uf[i] = i;
        }
        int[] count = new int[A.length];
        Arrays.fill(count, 1);
        Map<Integer, Integer> map = new HashMap<>();
        for(int i = 0; i < A.length; i++) {
            int val = A[i];
            for(int j = 2; j * j <= val; j++) {
                if(val % j == 0) {
                    if(map.containsKey(j)) {
                        union(uf, count, map.get(j), i);
                    } else {
                        map.put(j, i);
                    }
                    while(val % j == 0) val /= j;
                }
            }
            if(val != 1) {
                if(map.containsKey(val)) {
                    union(uf, count, map.get(val), i);
                } else {
                    map.put(val, i);
                }
            }
        }
        int ret = 0;
        for(int i = 0; i < count.length; i++) {
            ret = Math.max(count[i], ret);
        }
        return ret;
    }
}
```
721


sliding window 3

trie 3

segment tree 3

dp 3

binary search 3



class Solution {
    public void union(int[] uf, int x, int y) {
        int px = uf[x], py = uf[y];
        if(px == py) return;
        uf[py] = px;
    }
    public int find(int[] uf, int x) {
        int(uf[x] == x) {
            return x;
        }
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }
    public int makeConnected(int n, int[][] connections) {
        int[] uf = int[n];
        int[] count = new int[n];
        for(int i = 0 ; i < connections.length; i++) {
            union(uf, connections[0], connections[1]);
        }
        int tmp = 0;
        for(int i = 0; i < uf.length; i++) {
            if(uf[i] == i) tmp++;
        }
        return connections.length - n > tmp ? connections.length - n;
    }
}


1319. Number of Operations to Make Network Connected
```
class Solution {
    public void union(int[] uf, int x, int y) {
        int px = find(uf, x), py = find(uf, y);
        if(px == py) return;
        uf[py] = px;
    }
    public int find(int[] uf, int x) {
        if(uf[x] == x) {
            return x;
        }
        uf[x] = find(uf, uf[x]);
        return uf[x];
    }

    public int makeConnected(int n, int[][] connections) {
        int[] uf = new int[n];
        int[] count = new int[n];
        for(int i = 0; i < uf.length; i++) {
            uf[i] = i;
        }
        for(int i = 0 ; i < connections.length; i++) {
            union(uf, connections[i][0], connections[i][1]);
        }
        int tmp = 0;
        for(int i = 0; i < uf.length; i++) {
            //System.out.print(uf[i] + " ");
            if(uf[i] == i) tmp++;
        }
        return  connections.length + 1 < n ? -1 : tmp - 1;
    }
}
```


685. Redundant Connection II
```
class Solution {
    private int[] parent;
    private int[] root;
    private int[] size;
    public int[] findRedundantDirectedConnection(int[][] edges) {
        int N = edges.length;
        parent = new int[N + 1];
        root = new int[N + 1];
        int[] ans1 = null, ans2 = null;
        for(int i = 0; i < root.length; i++) root[i] = i;
        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            if(parent[v] > 0) {
                ans1 = new int[]{parent[v], v};
                ans2 = new int[]{u, v};
                edge[0] = edge[1] = -1;
            }
            parent[v] = u;
        }
        for(int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            if(u < 0 || v < 0) continue;
            int p = find(u);
            int q = find(v);
            if(p == q) return ans1 == null ? edge : ans1;
            root[p] = q;
        }
        return ans2;
    }
    private int find(int node) {
        if(root[node] == node) return node;
        root[node] = find(root[node]);
        return root[node];
    }
}
```
https://github.com/Seanforfun/Algorithm-and-Leetcode/blob/master/leetcode/685.%20Redundant%20Connection%20II.md



shortest path
433. Minimum Genetic Mutation
1129. Shortest Path with Alternating Colors 没写



863. All Nodes Distance K in Binary Tree
class Solution {
    public void bfs(TreeNode root, int k, List<Integer> ret) {
        if(root == null) return;
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        int step = 0;
        while(!queue.isEmpty()) {
            int size  = queue.size();
            int sz = 0;
            if(step == k) {
                while(!queue.isEmpty()) {
                    TreeNode cur = queue.poll();
                    ret.add(cur.val);
                }
                return;
            }
            while(sz < size) {
                TreeNode cur = queue.poll();
                if(cur.left != null) queue.add(cur.left);
                if(cur.right != null) queue.add(cur.right);
                sz++;
            }
            step++;
        }
        return;  
    }
    public void printNode(TreeNode root) {
        if(root == null) System.out.print("null ");
        else {
            System.out.print(root.val + " ");
            if(root.left == null) System.out.print("null ");
            else System.out.print(root.left.val + " ");
            if(root.right == null) System.out.println("null");
            else System.out.println(root.right.val);
        }
    }

    public int dfs(TreeNode root, TreeNode target, int k, List<Integer> ret) {
        if(root == null) return -1;
        if(root.val == target.val) {
            bfs(root, k, ret);
            return k - 1;
        }
        int step = dfs(root.left, target, k, ret);
        if(step == -1) {
            step = dfs(root.right, target, k, ret);
            if(step != -1) {
                root.right = null;
            }
        } else {
            root.left = null;
        }
        bfs(root, step, ret);
        if(step == -1) return -1;
        else return step - 1;
    }
    public List<Integer> distanceK(TreeNode root, TreeNode target, int K) {
        List<Integer> ret = new LinkedList<>();
        dfs(root, target, K, ret);
        return ret;
    }
}



shortest path with weighted
743. Network Delay Time
```
class Solution {
    public int networkDelayTime(int[][] times, int N, int K) {
        if(times == null || times.length == 0) return 0;
        int[][] graph = new int[N + 1][N + 1];

        for(int[] row : graph) Arrays.fill(row, -1);
        for(int i = 0; i < times.length; i++) {
           graph[times[i][0]][times[i][1]]= times[i][2];
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) ->(a[1] - b[1]));
        pq.offer(new int[]{K, 0});
        Set<Integer> visited = new HashSet<>();

        while(!pq.isEmpty()) {
            int[] cur = pq.poll();
            //if(visited.contains(cur[0])) continue;
            visited.add(cur[0]);
            if(visited.size() == N) return cur[1];
            for(int i = 1; i < graph[0].length; i++) {
                if(graph[cur[0]][i] == -1 || visited.contains(i)) continue;
                int dist = graph[cur[0]][i] + cur[1];
                pq.offer(new int[]{i, dist});
            }
        }
        return -1;
    }
}
```


787. Cheapest Flights Within K Stops
```
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int K) {
        if(flights == null || flights.length == 0) return 0;
        int[][] graph = new int[n + 1][n + 1];
        for(int[] row : graph) Arrays.fill(row, -1);
        for(int i = 0; i < flights.length; i++) {
           graph[flights[i][0]][flights[i][1]]= flights[i][2];
        }
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) ->(a[1] - b[1]));
        pq.offer(new int[]{src, 0, 0});
        Set<Integer> visited = new HashSet<>();
        while(!pq.isEmpty()) {
            int[] cur = pq.poll();
            if(cur[2] > K + 1) continue;
            visited.add(cur[0]);
            if(cur[0] == dst) return cur[1];
            for(int i = 0; i < graph[0].length; i++) {
                if(graph[cur[0]][i] == -1 || visited.contains(i)) continue;
                int dist = graph[cur[0]][i] + cur[1];
                pq.offer(new int[]{i, dist, cur[2] + 1});
            }
        }
        return -1;
    }
}
```

1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance
总结一下floyd 最短路径， 复杂度
```
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        int[][] dist = new int[n][n];
        int ret = 0, smallest = n;
        for(int[] row : dist) {
            Arrays.fill(row, 10001);
        }
        for(int[] e : edges) {
            dist[e[0]][e[1]] = dist[e[1]][e[0]] = e[2];
        }
        for(int i = 0; i < n; i++) dist[i][i] = 0;
        for(int k = 0; k < n; k++) {
            for(int i = 0; i < n; i++) {
                for(int j = 0; j < n; j++) {
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }
        for(int i = 0; i < n; i++) {
            int count = 0;
            for(int j = 0; j < n; j++) {
                if(dist[i][j] <= distanceThreshold) count++;
            }
            if(count <= smallest) {
                ret = i;
                smallest = count;
            }
        }
        return ret;
    }
}
```

882. Reachable Nodes In Subdivided Graph
还是很巧妙的。。。
```
class Solution {
    public int reachableNodes(int[][] edges, int M, int N) {
        int[][] graph = new int[N][N];
        for (int i = 0; i < N; i++) {
            Arrays.fill(graph[i], -1);
        }
        for(int[] edge : edges) {
            graph[edge[0]][edge[1]] = edge[2];
            graph[edge[1]][edge[0]] = edge[2];
        }
        int ret = 0;
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> (b[1] - a[1]));
        boolean[] visited = new boolean[N];
        pq.offer(new int[]{0, M});
        while(!pq.isEmpty()) {
            int[] cur = pq.poll();
            int start = cur[0];
            int move = cur[1];
            if(visited[start]) continue;
            visited[start] = true;
            ret++;
            for(int i = 0; i < N; i++) {
                if(graph[start][i] > -1) {
                    if(move > graph[start][i] && !visited[i]) {
                        pq.offer(new int[]{i, move - graph[start][i] - 1});
                    }
                    graph[i][start] -= Math.min(move, graph[start][i]);
                    ret += Math.min(move, graph[start][i]);
                }
            }
        }
        return ret;
    }
}
```



### 935
```
class Solution {
    // Solve problem in top down manner
    // Let F(i) be # of distinct numbers after i moves
    // Assume F(i') holds for all i' < i
    // F(i) is related to F(i-1) depends on where the chess is on step i. Let K be number of distinct choice at step i, we have
    // If chess is on 0, 1, 2, 3, 7, 8, 9, K = 2
    // If chess is on 4, 6, K = 3
    // If chess is on 5, K = 0
    // Base case: F(1) = 1

    private Integer[][] memo;
    private int[][] nextStep = {
            {4, 6},
            {6, 8},
            {7, 9},
            {4, 8},
            {3, 9, 0},
            {},
            {1, 7, 0},
            {2, 6},
            {1, 3},
            {4, 2},
    };

    public int knightDialer(int N) {
        this.memo = new Integer[10][N + 1];
        int count = 0;
        for (int i = 0; i <= 9; i++) {
            count += backtrack(i, N);
            count %= 1e9 + 7;
        }
        return count;
    }

    private int backtrack(int curNum, int N) {
        if (N == 1) {
            return 1;
        }

        if (memo[curNum][N] != null) {
            return memo[curNum][N];
        }

        memo[curNum][N] = 0;
        for (int nextNum : nextStep[curNum]) {
            memo[curNum][N] += backtrack(nextNum, N - 1);
            memo[curNum][N] %= (int)1e9 + 7;
        }
        return memo[curNum][N];
    }
}
```



class Solution {
    int ret = 0;
    public boolean valid(int[] count) {
        int flag = 0;
        for(int i = 0; i < count.length; i++) {
           if(count[i] % 2 != 0) {
               if(flag == 0) flag = 1;
               else return false;
           }
        }
        return true;
    }
    public void helper(TreeNode root, int[] count, List<Integer> last) {

        for(int e : count) System.out.print(e + " ");
        System.out.println("");
        if(root == null) return;

        if(root.left == null && root.right == null) {
            if(valid(count)) ret++;
            //count[root.val] = count[root.val]  - 1;
            return;
        }
        if(root.left != null) {
            count[root.left.val] = count[root.left.val]  + 1;
            last.add(root.left.val);
            helper(root.left, count, last);
            count[last.get(last.size() - 1)] = count[last.get(last.size() - 1)]  - 1;
            last.remove(last.size() - 1);
        }
        if(root.right != null) {
            count[root.right.val] = count[root.right.val]  + 1;
            last.add(root.right.val);
            helper(root.right, count, last);
            count[last.get(last.size() - 1)] = count[last.get(last.size() - 1)]  - 1;
            last.remove(last.size() - 1);
        }

    }
    public int pseudoPalindromicPaths (TreeNode root) {
        helper(root, new int[10], new ArrayList<>());
        return ret;
    }
}


### pending
heap 实现
quicksort 实现
gcd
union find
二分



```
public int stoneGameII(int[] piles) {
    int len = piles.length;
    int[] sum = new int[len + 1];
    int[][] dp = new int[len+1][len+1];//dp[i][M] : the best result a player can get from pile i to pile len - 1, with M as defined in the problem
    for(int i = len -1; i>=0; i--){
        sum[i] = sum[i+1] + piles[i];  
    }

    for(int i = len-1; i >= 0; i--){            
        for(int M = 1; M <= (len-i)/2; M++){
            int max = 0;
            for(int k = 1; k <= M *2; k++){
                // actually is (sum[i] - sum[i+k]) + (sum[i+k] - dp[i+k][Math.max(k, M)])
                // (sum[i] - sum[i+k]): choose the first K piles, start from i
                // dp[i+k][Math.max(k, M)]: max # of stones player 2 can get from remaining piles, with possibly changed M
                // (sum[i+k] - dp[i+k][Math.max(k, M)]): the best result player 1 can get after player 2 chooses. Both plays optimally.
                int cur = sum[i] - dp[i+k][Math.max(k, M)];
                max = Math.max(max, cur);
            }
            dp[i][M] = max;
        }
        for(int M = (len-i)/2 + 1; M<=len/2; M++){//len - i is the # of the remaining piles, if M is no less than half of the #, the player can take all the remaing piles (x <= 2*M)
            dp[i][M] = sum[i];
        }            
    }
    return dp[0][1];// start from pile 0, M = 1
}
```
class Solution {
    public int stoneGameII(int[] piles) {
        int[][] dp = new int[piles.length][300];
        int totalSum = 0;
        for (int i = 0; i < piles.length; i++) {
            totalSum += piles[i];
        }
        return findMax(piles, 0, dp, 1, totalSum);
    }

    int findMax(int[] piles, int i, int[][] dp, int M, int totalSum) {
        if (i >= piles.length) return 0;
        if (dp[i][M] > 0) return dp[i][M];
        int localMax = 0;
        int sum = 0;
        for (int j =i; j < Math.min(piles.length, i + 2 * M); j++) {
            sum += piles[j];
            localMax = Math.max(localMax, totalSum - findMax(piles, j + 1, dp, Math.max(M, j + 1 - i), totalSum - sum));
        }
        dp[i][M] = localMax;
        return dp[i][M];
    }
}
```



class Solution {
    public int helper(int[] piles, int[][] memo, int index, int M, int[] sum) {
        if(index == piles.length) return 0;
        if(2 * M >= piles.length - index) return sum[index];
        if(memo[index][M] != -1) return memo[index][M];


        int ret = 0;
        for(int i =  1 ; i <= M * 2; i++) {
            ret = Math.max(ret,sum[index] - helper(piles, memo, index + i, Math.max(M, i), sum));
        }
        memo[index][M] = ret;
        return ret;
    }

    public int stoneGameII(int[] piles) {
        int n = piles.length;
        int[] sum = new int[n];
        sum[n - 1] = piles[n - 1];
        for(int i = piles.length - 2; i >= 0; i--) {
            sum[i] +=  sum[i + 1] + piles[i];
        }
        int[][] memo = new int[n][n];
        for(int[] me : memo) Arrays.fill(me, -1);
        return helper(piles, new int[n][n], 0, 1, sum);
    }
}
```



```
class Solution {
    public int minCost(int[] houses, int[][] cost, int m, int n, int target) {
        int[][][] dp = new int[1 + m][1 + n][1 + target];
        for (int i = 0; i <= m; i++) {
            for (int j = 0; j <= n; j++) {
                for (int k = 0; k <= target; k++) {
                    dp[i][j][k] = Integer.MAX_VALUE;
                }
            }
        }
        dp[0][0][0] = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j <= n; j++) {
                for (int k = 0; k <= target; k++) {
                    if (dp[i][j][k] == Integer.MAX_VALUE) {
                        continue;
                    }
                    if (houses[i] == 0) {
                        // paint house at i
                        for (int nj = 1; nj <= n; nj++) {
                            if (nj == j) {
                                dp[i + 1][j][k] = Math.min(dp[i + 1][j][k], dp[i][j][k] + cost[i][nj - 1]);
                            } else {
                                if (k != target) {
                                    dp[i + 1][nj][k + 1] = Math.min(dp[i + 1][nj][k + 1], dp[i][j][k] + cost[i][nj - 1]);
                                }
                            }
                        }
                    } else {
                        // no paint at i
                        int nj = houses[i];
                        if (nj == j) {
                            dp[i + 1][j][k] = Math.min(dp[i + 1][j][k], dp[i][j][k]);
                        } else {
                            if (k != target) {
                                dp[i + 1][nj][k + 1] = Math.min(dp[i + 1][nj][k + 1], dp[i][j][k]);
                            }
                        }
                    }
                }
            }
        }

        int res = Integer.MAX_VALUE;
        for (int j = 1; j <= n; j++) {
            res = Math.min(res, dp[m][j][target]);
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
```


1. treemap 复杂度
2. 快速排序
