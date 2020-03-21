# 62. Unique Paths

## Problem Description

A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?



## Solution

### Dynamic Programming

We can use a 2D DP array to store the possible paths at each grid. To reach the grid at DP\[a][b], it must go through DP\[a-1][b] or DP\[a][b-1]. So the count can be determined by adding the two former counts. On the first row and the first column, all the grids can be reached only in one path. So, they should be initially assigned 1.

This method can be optimized in space complexity. We can use an array to store the grids in a row. As we scan all the rows, we update the array in order. And when it comes to the end, we return the last  value in the array.

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<int> grids(n,1);
        for(int i=1; i<m; i++){
            for(int j=1; j<n; j++){
                grids[j] += grids[j-1];
            }
        }
        return grids[n-1];
    }
};
```

