# Coin Change

### Description

You are given coins of different denominations and a total amount of money *amount*. Write a function to compute the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

### Solutions

##### Method 1: Dynamic programming - Top down

First, let's define:

> *F(S)* - minimum number of coins needed to make change for amount *S* using coin denominations [*c<sub>0</sub> ... c<sub>n-1</sub>*]

We note that this problem has an optimal substructure property, which is the key piece in solving any Dynamic Programming problems. In other words, the optimal solution can be constructed from optimal solutions of its subproblems. How to split the problem into subproblems? Let's assume that we know *F(S)* where some change *val<sub>1</sub>, val<sub>2</sub>, ...* for *S* which is optimal and the last coin's denomination is *C*. Then the following equation should be true because of optimal substructure of the problem:

<img src="..\images\0322\1.png" alt="1" style="zoom:80%;" />

But we don't know which is the denomination of the last coin *C*. We compute *F( S - c<sub>i</sub> )* for each possible denomination *c<sub>0</sub> ... c<sub>n-1</sub>* and choose the minimum among them. The following recurrence relation holds:

<img src="..\images\0322\2.png" alt="2" style="zoom:80%;" />

![3](..\images\0322\3.png)

In the recursion tree above, we could see that a lot of subproblems were calculated multiple times. For example the problem *F*(1) was calculated 13 times. Therefore we should cache the solutions to the subproblems in a table and access them in constant time when necessary

##### Code:

```java
public class Solution {

  public int coinChange(int[] coins, int amount) {
    if (amount < 1) return 0;
    return coinChange(coins, amount, new int[amount]);
  }

  private int coinChange(int[] coins, int rem, int[] count) {
    if (rem < 0) return -1;
    if (rem == 0) return 0;
    if (count[rem - 1] != 0) return count[rem - 1];
    int min = Integer.MAX_VALUE;
    for (int coin : coins) {
      int res = coinChange(coins, rem - coin, count);
      if (res >= 0 && res < min)
        min = 1 + res;
    }
    count[rem - 1] = (min == Integer.MAX_VALUE) ? -1 : min;
    return count[rem - 1];
  }
}
```

------

##### Method 2: Dynamic programming - Bottom up

For the iterative solution, we think in bottom-up manner. Before calculating *F*(i), we have to compute all minimum counts for amounts up to *i*. On each iteration *i* of the algorithm *F*(i) is computed as 

min<sub>j=0, ..., n-1</sub> *F*( i - c<sub>j</sub> ) + 1

<img src="..\images\0322\4.png" alt="4" style="zoom:80%;" />

##### Code:

```java
public class Solution {
  public int coinChange(int[] coins, int amount) {
    int max = amount + 1;
    int[] dp = new int[amount + 1];
    Arrays.fill(dp, max);
    dp[0] = 0;
    for (int i = 1; i <= amount; i++) {
      for (int j = 0; j < coins.length; j++) {
        if (coins[j] <= i) {
          dp[i] = Math.min(dp[i], dp[i - coins[j]] + 1);
        }
      }
    }
    return dp[amount] > amount ? -1 : dp[amount];
  }
}
```

