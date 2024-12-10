# 一、题目

```
Wonderland是小王居住地一家很受欢迎的游乐园。
Wonderland目前有4种售票方式，
分别为一日票（1天）、三日票（3天）、
周票（7天）和月票（30天）。

每种售票方式的价格将由一个数组给出，
每种票据在票面时限内可以无限制的进行游玩。
例如，小王在第10日买了一张三日票，
小王可以在第10日、第11日和第12日进行
无限制的游玩。

小王计划在接下来一年内多次游玩该游乐园。
小王计划的游玩日期将由一个数组给出。
现在，请您根据给出的售票价格数组和
小王计划游玩日期数组，
返回完成游玩计划所需要的最低消费.
```

# 二、输入
```
输入为2个数组

售票价格数組为costs, costs.length=4，
默认顺序为一日票、三日票、周票和月票。

小王计划游玩日期数组为days,
1<=days.length<=365,
1<=days[i] <=365，，默认顶序为升序。
```

# 三、输出
```
完成游玩计划的最低消费
```

# 四、示例
```
输入：
5 14 30 100
1 3 15 20 21 200 202 230

输出：
40

说明：
根据售票价格数组和游玩日期数组给出的信息，
发现每次去玩的时候买一张一日票是最省钱的，
所以小王会买8张一日票，每张5元，
最低花费是40元
```

# 五、题解
1. 题目中求**最低花费**，考虑有最优子结构使用动态规划算法
2. dp[i]定义为从第 i 天到年终的最低花费。这样考虑当天买单日票，3 天票更好理解

## 5.1 Java 实现

```Java
package org.stone.study.algo.ex202411;

import java.util.Arrays;
import java.util.Scanner;

public class Wonderland {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        // 5 14 30 100
        String[] line1 = scanner.nextLine().split(" ");
        int[] costs = Arrays.stream(line1).mapToInt(Integer::parseInt).toArray();
        // 1 3 15 20 21 200 202 230
        String[] line2 = scanner.nextLine().split(" ");
        int[] days = Arrays.stream(line2).mapToInt(Integer::parseInt).toArray();

        int minCost = calcMinCost(costs, days);
        System.out.println(minCost);
    }

    /**
     * 计算最小花费:动态规划，反向计算更容易理解
     * @param costs
     * @param days
     * @return
     */
    private static int calcMinCost(int[] costs, int[] days) {
        boolean[] visited = new boolean[366];
        Arrays.fill(visited, false);
        for(int day : days) {
            visited[day] = true;
        }

        // dp[i] 表示从第i天开始，到365天结束，需要的最小花费
        int[] dp = new int[366];
        // dp值初始化：如果最后一天不需要旅游，花费为0，否则花费为1天票
        dp[365] = visited[365] ? costs[0] : 0;
        // 反向计算更容易理解
        for (int i = 364; i >= 1; --i) {
            if(!visited[i]) {
                dp[i] = dp[i+1];
                continue;
            }

            dp[i] = dp[i + 1] + costs[0]; // 买 1 天票
            dp[i] = Math.min(dp[i], dp[Math.min(i + 3, 365)] + costs[1]); // 买 3 天票
            dp[i] = Math.min(dp[i], dp[Math.min(i + 7, 365)] + costs[2]); // 买 7 天票
            dp[i] = Math.min(dp[i], dp[Math.min(i + 30, 365)] + costs[3]); // 买 30 天票
        }

        return dp[1];
    }
}
```

## 5.2 Python实现

```Python
# 动态规划反向计算最小代价
def calcMinCost(costs, days):
    visited = [False] * 366

    for day in days:
        visited[day] = True

    # dp[i]表示从第i天到最后一天的最小代价。这里初始化值没有影响，因为遍历的过程不依赖这个初值，会被更新。
    dp = [0] * 366
    dp[365] = costs[0] if visited[365] else 0
    for i in range(364, 0, -1):
        if not visited[i]:
            dp[i] = dp[i + 1]
            continue

        dp[i] = dp[i + 1] + costs[0]
        dp[i] = min(dp[i], dp[min(i + 3, 365)] + costs[1])
        dp[i] = min(dp[i], dp[min(i + 7, 365)] + costs[2])
        dp[i] = min(dp[i], dp[min(i + 30, 365)] + costs[3])

    return dp[1]

if __name__ == "__main__":
    costs = list(map(int, input().split()))
    days = list(map(int, input().split()))
    min_cost = calcMinCost(costs, days)

    print(min_cost)
```