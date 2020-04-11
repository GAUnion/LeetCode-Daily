# Burst ballons
**Problem** Given n balloons, indexed from 0 to n-1. Each balloon is painted with a number on it represented by array nums. You are asked to burst all the balloons. If the you burst balloon i you will get nums[left] * nums[i] * nums[right] coins. Here left and right are adjacent indices of i. After the burst, the left and right then becomes adjacent.    

Find the maximum coins you can collect by bursting the balloons wisely.    

Note:    

    You may imagine nums[-1] = nums[n] = 1. They are not real therefore you can not burst them.  
    0 ≤ n ≤ 500, 0 ≤ nums[i] ≤ 100    
  
Example:    
  
Input: [3,1,5,8]  
Output: 167   
Explanation: nums = [3,1,5,8] --> [3,5,8] -->   [3,8]   -->  [8]  --> []  
             coins =  3*1*5      +  3*5*8    +  1*3*8      + 1*8*1   = 167   
  
## DP(top-down)
If we simply calculate all the possible sequence, the complexity is O(N!)  
Optimization:  
1. Divde and conquer  
The question is divided into func(nums\[0:i\]) and func(nums\[i+1:0\]) after burst the ballon i.   
Firstly, in order to hit the optimum, we need to find the local optimum after we burst every ballon. We need to burst every ballon in a certain section to find the local optimum, thus the complexity is O(N) ∗ O(N2)=O(N3).  
2. From back to front  
We are facing the adjacent problem whenever we burst a single ballon. To solve it, we can do it from back to front: start with no ballon and add a ballon at a time. We first divide and conquer the left part and the right part of this ballon and then add the ballon to the middle of two parts.  Add ballon i can get coins: nums\[left\] * nums\[i\] * nums\[right\]    
Algorithm:  
To deal with border of arrary, we add 1 to the leftmost and rightmost of arrary. And find the max coins we get when we only burst the ballons between the leftmost and rightmost.  
Define function dp, which return the most coins from (left, right). When left + 1 == right, dp\[left\]\[right\] = 0. Then, add ballon in the arrary and divide the problem into the right part and the left part to find the most coins  
The max coins after having added the ballon i: nums\[left\] * nums\[i\] * nums\[right\] + dp(left, i) + dp(i, right). nums\[left\] * nums\[i\] * nums\[right\] is the coins got from adding the ballon i, dp(left, i) + dp(i, right) is the max coins got from the right and left parts.  

```python
from functools import lru_cache

class Solution:
    def maxCoins(self, nums: List[int]) -> int:

        # reframe the problem
        nums = [1] + nums + [1]

        # cache this
        @lru_cache(None)
        def dp(left, right):

            # no more balloons can be added
            if left + 1 == right: return 0

            # add each balloon on the interval and return the maximum score
            return max(nums[left] * nums[i] * nums[right] + dp(left, i) + dp(i, right) for i in range(left+1, right))

        # find the maximum number of coins obtained from adding all balloons from (0, len(nums) - 1)
        return dp(0, len(nums)-1)
```

Time comlexity: O(N3)  
Space comlexity: O(N2)  

## DP(bottom-up)

```python
class Solution:
    def maxCoins(self, nums: List[int]) -> int:

        # reframe problem as before
        nums = [1] + nums + [1]
        n = len(nums)

        # dp will store the results of our calls
        dp = [[0] * n for _ in range(n)]

        # iterate over dp and incrementally build up to dp[0][n-1]
        for left in range(n-2, -1, -1):
            for right in range(left+2, n):
                # same formula to get the best score from (left, right) as before
                dp[left][right] = max(nums[left] * nums[i] * nums[right] + dp[left][i] + dp[i][right] for i in range(left+1, right))

        return dp[0][n-1]
```

