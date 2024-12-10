# 一、题目

```
Leetcode 3258题：

给你一个 二进制 字符串 s 和一个整数 k。

如果一个 二进制字符串 满足以下任一条件，
则认为该字符串满足 k 约束：
1. 字符串中 0 的数量最多为 k。
2. 字符串中 1 的数量最多为 k。
返回一个整数，表示 s 的所有满足 k 约束 
的子字符串的数量。
```

# 二、输入
```
s 和 k
```

# 三、输出
```
返回一个整数，表示 s 的所有满足 k 约束 
的子字符串的数量。
```

# 四、示例
示例一
```
输入：
s = "10101", k = 1

输出：
12
```
* 解释：s 的所有子字符串中，除了 "1010"、"10101" 和 "0101" 外，其余子字符串都满足 k 约束。

示例二
```
输入：
s = "1010101", k = 2

输出：
25
```

# 五、题解
1. 这题是 LeetCode 简单题，实际做起来不简单
2. 先理解题意**子字符串**是连续的，满足任一个最多 K 个字符的条件
3. 一开始往排列组合集合方向想就复杂了
4. 暴力方法是两重遍历，参考下面 Java 解法
5. 优化的方法是滑动窗口，参考下面的 Python 解法。统计方法是**针对每一个右边界位置，在满足题目条件时，累加符合条件的左边界字符个数**，也就是 `right - left + 1 `

## 5.1 Java 实现

```Java
class Solution {
    public int countKConstraintSubstrings(String s, int k) {
        int n = s.length();
        int ans = 0;
        for(int i = 0; i < n; i++) {
            int[] cnt = new int[2];
            cnt[0] = 0;
            cnt[1] = 0;
            for(int j = i; j < n; j++) {
                cnt[s.charAt(j) - '0']++;

                if(cnt[0] > k && cnt[1] > k) {
                    break;
                }
                ++ans;
            }
        }

        return ans;
    }
}
```

## 5.2 Python实现

```Python
class Solution:
    def countKConstraintSubstrings(self, s: str, k: int) -> int:
        ans, left = 0, 0
        cnt = [0, 0]
        for right, c in enumerate(s):
            cnt[int(c)] += 1
            while cnt[0] > k and cnt[1] > k:
                cnt[int(s[left])] -= 1
                left += 1
                
            # 以right为右边界返回子字符串的个数
            ans += (right - left + 1)

        return ans
    
obj = Solution()
ans = obj.countKConstraintSubstrings("10101", 1)
# 12
print(ans)
ans = obj.countKConstraintSubstrings("1010101", 2)
# 25
print(ans)
ans = obj.countKConstraintSubstrings("11111", 1)
# 15
print(ans)
```