# Maximal Square

### Problem

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing all 1's and return its area.

### Solution

This is a typical dynamic programming problem. We establish a 2D matrix dp\[m]\[n], where m and n are the height and width of the original 2D matrix respectively.

dp\[i]\[j] represents the length of the largest square whose end point is the bottom right corner of the matrix\[i]\[j]. If matrix\[i]\[j] == '0', the end point cannot form a square, and the side length is 0.

Each point in the matrix is the side length of the largest square that can be formed by the end point of the lower right corner of the square, or the minimum value of the side length of the largest square that can be formed by the left, upper and upper left adjacent ends of the point plus 1 (the point is '1'), or 0 (the point is '0').

Dynamic transfer equation:

dp\[i]\[j] = min(dp\[i-1]\[j], dp\[i]\[j-1]], dp\[i-1]\[j-1]) (the point is '1')

dp\[i]\[j] = 0 (the point is '0')

```
class Solution:
    def maximalSquare(self, matrix: List[List[str]]) -> int:
        m = len(matrix)
        if m==0:return 0
        n = len(matrix[0])
        dp = [[0 for i in range(n)] for i in range(m)]
        
        ans = 0
        for i in range(m):
            for j in range(n):
                if i==0 or j==0:
                    dp[i][j] = int(matrix[i][j])
                else:
                    if matrix[i][j]!='0':
                        dp[i][j] = 1 + min(dp[i-1][j],dp[i][j-1],dp[i-1][j-1])
                    else:
                        dp[i][j]=0
                ans = max(ans,dp[i][j])                
        return ans**2
```

