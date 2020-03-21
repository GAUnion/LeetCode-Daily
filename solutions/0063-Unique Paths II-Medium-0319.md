# 63. Unique Paths II

## Problem Description

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

Now consider if some obstacles are added to the grids. How many unique paths would there be?

![avatar](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

An obstacle and empty space is marked as 1 and 0 respectively in the grid.


## Solutions

### 1. Dynamic Programming 

The robot can only move either down or right. Hence any cell in the first row can only be reached from the cell left to it. However, if any cell has an obstacle, you don't let that cell contribute to any path. So, for the first row, the number of ways will simply be
if obstacleGrid[i][j] is not an obstacle
     obstacleGrid[i,j] = obstacleGrid[i,j - 1] 
else
     obstacleGrid[i,j] = 0
You can do a similar processing for finding out the number of ways of reaching the cells in the first column.

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        vector<vector<long>> path(obstacleGrid.size(), vector<long>(obstacleGrid[0].size(), 0));
        for (int i=0; i<obstacleGrid.size(); i++) {
            for (int j=0; j<obstacleGrid[0].size(); j++) {
                if (obstacleGrid[i][j]) {
                    continue;
                } else {
                    if (i>0 && j>0) {
                        path[i][j] += path[i-1][j] + path[i][j-1];
                    } else if (i==0 && j==0) {
                        path[i][j] = 1;
                    } else if (i==0) {
                        path[i][j] += path[i][j-1];
                    } else {
                        path[i][j] += path[i-1][j];
                    }
                }
            }
        }
        return path[obstacleGrid.size()-1][obstacleGrid[0].size()-1];
    }
};
```

***

### 2. Dynamic Programming (less code)

Add an obstacle to the left and top so that you don't have to judge the boundaries.

```python
class Solution:
    def uniquePathsWithObstacles(self, obstacleGrid: List[List[int]]) -> int:
        m, n = len(obstacleGrid[0]), len(obstacleGrid)
        dp = [1] + [0]*m
        for i in range(0, n):
            for j in range(0, m):
                dp[j] = 0 if obstacleGrid[i][j] else dp[j]+dp[j-1]
        return dp[-2]
```



