# 474. Ones and Zeroes

## Problem

Given an array, `strs`, with strings consisting of only `0s` and `1s`. Also two integers `m` and `n`.

Now your task is to find the maximum number of strings that you can form with given **m** `0s` and **n** `1s`. Each `0` and `1` can be used at most **once**.

**Example 1:**

```
Input: strs = ["10","0001","111001","1","0"], m = 5, n = 3
Output: 4
Explanation: This are totally 4 strings can be formed by the using of 5 0s and 3 1s, which are "10","0001","1","0".
```

**Example 2:**

```
Input: strs = ["10","0","1"], m = 1, n = 1
Output: 2
Explanation: You could form "10", but then you'd have nothing left. Better form "0" and "1".
```

## Solution

This problem we can see is a `0-1 backpack` issue, either take current string or not take current string.

Define an array dp [m+1] [n+1], representing the maximum counts,  m is the number of 0 and n is the number of 1. So we can get the DP formula as：

```
dp[i][j] = max(dp[i][j], dp[i - count0][j - count1] + 1)
```

**Python3 Code**

```python
class Solution:
    def findMaxForm(self, strs: List[str], m: int, n: int) -> int:
        dp = [[0] * (n + 1) for _ in range(m + 1)]
        for str in strs:
            count0, count1 = 0, 0
            for c in str:
                if c == "0":
                    count0 += 1
                elif c == "1":
                    count1 += 1
            for i in range(m, count0 - 1, -1):
                for j in range(n, count1 - 1, -1):
                    dp[i][j] = max(dp[i][j], dp[i-count0][j-count1] + 1)
        return dp[m][n]
```

**C++ Code**

```python
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (string str : strs) {
            int count0 = 0, count1 = 0;
            for (char c : str) (c == '0') ? ++count0 : ++count1;
            for (int i = m; i >= count0; --i) {
                for (int j = n; j >= count1; --j) {
                    dp[i][j] = max(dp[i][j], dp[i - count0][j - count1] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

- Time Complexity: O(l * m * n) 

  l is the length of strs，m is the number of 0，n is the number of 1

- Space Complexity: O(m * n) 