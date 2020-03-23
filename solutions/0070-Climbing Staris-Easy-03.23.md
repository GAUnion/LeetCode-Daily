# Climbing Stairs

### Method1: Dynamic Programming


It is obvious that dp[i] = dp[i - 1] + dp[i - 2] because the steps include only 1 and 2. Then assign dp[1] and dp[0] as 1.


```python
class Solution:
    def climbStairs(self, n: int) -> int:
        pre = 1
        cur = 1
        for i in range(2, n + 1):
            pre, cur = cur, pre + cur
        return cur
```

### Method2: Febonacci Arrary

After implementing method one, we can tell that it is the same as Febonacci Arrary.

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        import math
        sqrt5=5**0.5
        fibin=math.pow((1+sqrt5)/2,n+1)-math.pow((1-sqrt5)/2,n+1)
        return int(fibin/sqrt5)
```

