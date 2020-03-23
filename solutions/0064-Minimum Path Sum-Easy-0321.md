# Minimum Path Sum

### Description

Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.

**Note:** You can only move either down or right at any point in time.


## Dynamic Programming

This problem is a very typical Dynamic Programming(DP) problem. The DP method is to store form solutions in the DP array while traversing the grid. The key to DP is finding the initial state and state transition equation.
In this problem, the initial state is grid[0,0], i.e.top left corner, and there are two paths to reach the current point -- up and left. So the minimum sum is the value of current point + min(minimum path sum of the top point, minimum path sum of the left point). Moreover, in this problem, there are two ways of DP-- one dimensional and two dimensional:

### 2D-DP
```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        if not grid:
            return 0
        m, n = len(grid[0]), len(grid) 
        dp = grid
        for i in range(1,m):
            dp[0][i] = dp[0][i-1]+grid[0][i]
        for i in range(1,n):
            dp[i][0] = dp[i-1][0]+grid[i][0]
        for i in range (1,n):
            for j in range(1,m):
                dp[i][j] += min(dp[i-1][j], dp[i][j-1])
        return dp[-1][-1]
```

### 1D-DP

```python
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        dp = [0 for _ in range(len(grid[0]))]
        for i in range(len(grid)):
            for j in range(len(grid[0])):
                if i == 0 and j == 0:
                    dp[j] = grid[i][j]
                elif i == 0 and j != 0:
                    dp[j] = dp[j - 1] + grid[i][j]
                elif i != 0 and j == 0:
                    dp[j] = dp[j] + grid[i][j]
                else:
                    dp[j] = min(dp[j], dp[j - 1]) + grid[i][j]
        return dp[-1]
```

