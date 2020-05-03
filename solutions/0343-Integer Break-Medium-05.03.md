# 343. Integer Break

### Description

Given a positive integer *n*, break it into the sum of **at least** two positive integers and maximize the product of those integers. Return the maximum product you can get.

**Example 1:**

```
Input: 2
Output: 1
Explanation: 2 = 1 + 1, 1 × 1 = 1.
```

**Example 2:**

```
Input: 10
Output: 36
Explanation: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36.
```

**Note**: You may assume that *n* is not less than 2 and not larger than 58.

### Math behind this problem

**Why the factor can only be 2 or 3?**

If an optimal product contains a factor `f >= 4`, then you can replace it with factors `2` and `f-2` without losing optimality, as `2*(f-2) = 2f-4 >= f`. So you never need a factor greater than or equal to 4, meaning you only need factors 1, 2 and 3 (and 1 is of course wasteful and you'd only use it for n=2 and n=3, where it's needed).

For the rest I agree, 3$\times$3 is simply better than 2$\times$2$\times$2, so you'd never use 2 more than twice.

**(More complicated version)** For convenience, say **n** is sufficiently large and can be broken into any smaller real positive numbers. We now try to calculate which real number generates the largest product.
Assume we break **n** into **(n / x)** **x**'s, then the product will be **xn/x**, and we want to maximize it.

Taking its derivative gives us **n \* xn/x-2 \* (1 - ln(x))**.
The derivative is positive when **0 < x < e**, and equal to **0** when **x = e**, then becomes negative when **x > e**,
which indicates that the product increases as **x** increases, then reaches its maximum when **x = e**, then starts dropping.

This reveals the fact that if **n** is sufficiently large and we are allowed to break **n** into real numbers,
the best idea is to break it into nearly all **e**'s.
On the other hand, if **n** is sufficiently large and we can only break **n** into integers, we should choose integers that are closer to **e**.
The only potential candidates are **2** and **3** since **2 < e < 3**, but we will generally prefer **3** to **2**. Why?

Of course, one can prove it based on the formula above, but there is a more natural way shown as follows.

**6 = 2 + 2 + 2 = 3 + 3**. But **2 \* 2 \* 2 < 3 \* 3**.
Therefore, if there are three **2**'s in the decomposition, we can replace them by two **3**'s to gain a larger product.

All the analysis above assumes **n** is significantly large. When **n** is small (say **n <= 10**), it may contain flaws.
For instance, when **n = 4**, we have **2 \* 2 > 3 \* 1**.
To fix it, we keep breaking **n** into **3**'s until **n** gets smaller than **10**, then solve the problem by brute-force.

### Solutions

C++ implementation of iterative solution: O(n) time

```
public class Solution {
    public int integerBreak(int n) {
        if(n==2) return 1;
        if(n==3) return 2;
        int product = 1;
        while(n>4){
            product*=3;
            n-=3;
        }
        product*=n;
        
        return product;
    }
}
```

O($\log n $) solution: because pow() cause $\log n$ time

```
class Solution {
public:
    int integerBreak(int n) {
        if (n==2) return 1;
        if (n==3) return 2;
        
        int times = n/3;
        int remainder = n%3;
        if (remainder == 1) { times--; remainder = 4;}
        else if (remainder == 0) remainder = 1;
        return pow(3, times) * remainder;
    }
};
```

