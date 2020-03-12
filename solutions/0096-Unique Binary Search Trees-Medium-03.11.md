# Unique Binary Search Trees

### Problem:

Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1 ... *n*?

### Solution:

It's a typical dynamic programming problem.

First, we establish the state int [] dp = new int[n+1] to store the results of 0~n nodes.

Then we give the initial state, when n = 0 or 1, there is only one unique **BTS's**, i.e. dp[0] = dp[1] =1.

Third, we need to find the transformation function:

​	for node n, each node can be the root:

​		for each root k, its left branch has k - 1 nodes, its right branch has n - k nodes:

​			the state of n should be the sum of all the products of the two branch of each root.

### code:

```python
class Solution {
    public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; i++) {
            for(int root = 1; root <= i; root++) {
                int left = root - 1;
                int right = i - root;
                dp[i] += dp[left] * dp[right];
            }
        }
        return dp[n];
    }
}
```



