# 一、题目

```
给你一个整数 n 和一个二维整数数组 queries。

有 n 个城市，编号从 0 到 n - 1。
初始时，每个城市 i 都有一条单向道路通往城市 i + 1
（ 0 <= i < n - 1）。

queries[i] = [ui, vi] 表示新建一条
从城市 ui 到城市 vi 的单向道路。
每次查询后，你需要找到从城市 0 
到城市 n - 1 的最短路径的长度。

返回一个数组 answer，
对于范围 [0, queries.length - 1] 
中的每个 i，answer[i] 是处理完前 i + 1 
个查询后，从城市 0 到城市 n - 1 
的最短路径的长度。
```

# 二、输入
```
城市数和查询数组
```

# 三、输出
```
返回一个数组 answer，
对于范围 [0, queries.length - 1] 
中的每个 i，answer[i] 是处理完前 i + 1 
个查询后，从城市 0 到城市 n - 1 
的最短路径的长度。
```

# 四、示例
* 示例一
```
输入：
n = 5, queries = [[2, 4], [0, 2], [0, 4]]

输出：
[3, 2, 1]
```

* 示例二
```
输入：
n = 4, queries = [[0, 3], [0, 2]]

输出：
[1, 1]
```

# 五、题解

## 5.1 Java 实现

```Java
package org.stone.study.algo.ex202411;

import java.util.*;

/**
 * 给你一个整数 n 和一个二维整数数组 queries。
 * 有 n 个城市，编号从 0 到 n - 1。
 * 初始时，每个城市 i 都有一条单向道路通往城市 i + 1
 * （ 0 <= i < n - 1）。
 * queries[i] = [ui, vi] 表示新建一条
 * 从城市 ui 到城市 vi 的单向道路。
 * 每次查询后，你需要找到从城市 0
 * 到城市 n - 1 的最短路径的长度。
 * 返回一个数组 answer，
 * 对于范围 [0, queries.length - 1]
 * 中的每个 i，answer[i] 是处理完前 i + 1
 * 个查询后，从城市 0 到城市 n - 1
 * 的最短路径的长度。
 */
public class ShortestPaths {
    public static void main(String[] args) {
        int n = 5;
        int[][] queries = {{2, 4}, {0, 2}, {0, 4}};
//        int[] ans = new ShortestPaths().shortestDistanceAfterQueries(n, queries);
        int[] ans = new ShortestPaths().shortestDistanceAfterQueries2(n, queries);
        // [3, 2, 1]
        System.out.println(Arrays.toString(ans));
    }
    public int[] shortestDistanceAfterQueries(int n, int[][] queries) {
        // 初始化邻接表
        List<Integer>[] neighbors = new ArrayList[n];
        for(int i = 0; i < n - 1; i++) {
            neighbors[i] = new ArrayList<>();
            neighbors[i].add(i + 1);
        }
        //避免空指针，最后一个也初始化
        neighbors[n-1] = new ArrayList<>();

        // 每增加一条边重新计算一次最小路径
        int m = queries.length;
        int[] ans = new int[m];
        for(int i = 0; i < m; i++) {
            int[] query = queries[i];
            neighbors[query[0]].add(query[1]);
            ans[i] = bfs(neighbors);
        }

        return ans;
    }

    // 广度遍历求最小路径
    private int bfs(List<Integer>[] neighbors) {
        int n = neighbors.length;
        int[] dist = new int[n];
        Arrays.fill(dist, -1);

        Queue<Integer> queue = new LinkedList<>();
        queue.offer(0);
        dist[0] = 0;
        while(!queue.isEmpty()) {
            int cur = queue.poll();
            for(int neighbor : neighbors[cur]) {
                // 已经计算过的一定是最小的，不用对 dist 进行比较
                if (dist[neighbor] < 0) {
                    queue.offer(neighbor);
                    dist[neighbor] = dist[cur] + 1;
                }
            }
        }

        return dist[n - 1];
    }

    // 动态规划解法
    public int[] shortestDistanceAfterQueries2(int n, int[][] queries) {
        int[] ans = new int[queries.length];
        int[] dp = new int[n]; // 存储从 0 到 节点i 的最短路径长度
        ArrayList<Integer>[] preNodes = new ArrayList[n]; // 存储从 0 到 i 的所有前驱节点
        for (int i = 1; i < n; i++) {
            dp[i] = i;
            preNodes[i] = new ArrayList<Integer>();
            preNodes[i].add(i-1);
        }

        for (int i = 0; i < queries.length; i++) {
            int u = queries[i][0], v = queries[i][1];
            preNodes[v].add(u);
            // 只需要更新从 v 开始到 n-1 的最短路径
            for(int j = v; j < n; j++) {
                for(int pre : preNodes[j]) {
                    if (dp[pre] + 1 < dp[j]) {
                        dp[j] = dp[pre] + 1;
                    }
                }
            }
            // 本轮的最短路径
            ans[i] = dp[n-1];
        }

        return ans;
    }
}


```

## 5.2 Python实现

```Python

def shortPath(n, queries):
    # pre[i] 表示i 的所有前驱节点
    pre = [[i - 1] for i in range(n)]
    pre[0] = []
    # dp[i] 表示从 0 到 i 的最短距离
    dp = [i for i in range(n)]
    ans = []
    for (u, v) in queries:
        pre[v].append(u)
        for j in range(v, n):
            for k in pre[j]:
                # 从前驱节点中更新最短距离
                dp[j] = min(dp[j], dp[k] + 1)
        ans.append(dp[n - 1])

    return ans


if __name__ == '__main__':
    n = 5
    queries = [[2, 4], [0, 2], [0, 4]]
    ans = shortPath(n, queries)
    print(ans)
```