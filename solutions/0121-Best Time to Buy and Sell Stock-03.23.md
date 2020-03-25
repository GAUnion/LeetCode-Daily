### LeetCode 121 [ Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

------

#### Problem

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example:

```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Not 7-1 = 6, as selling price needs to be larger than buying price.

```

#### Solution 1: Dynamic Programming

------

1. Record the lowest price before today.
2. Decide whether today price minus lowest price is smaller than the previous max profit.
3. Return the max profit.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int max_profit = 0;
        int low_price = 65535;
        for (int i = 0; i < prices.size();i++) {
            max_profit = max(max_profit,prices[i] - low_price);
            low_price = min(low_price, prices[i]);
        }
        return max_profit;
    }
};
```

#### Solution 2: Brute Force

------

Simply figure out the largest substraction result by using 2 loops to calculate all the results.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = (int)prices.size(), ans = 0;
        for (int i = 0; i < n; ++i){
            for (int j = i + 1; j < n; ++j){
                ans = max(ans, prices[j] - prices[i]);
            }
        }
        return ans;
    }
};
```

#### Solution 3: Pointers

------

Use two pointers to replace the loops to reduce time complexity.

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()<2) return 0;
        int res = 0;
        int left=0, right=1;
        
        while(right < prices.size()){
            if(prices[left] > prices[right]){
                left = right;
            }else if(prices[left] < prices[right]){
                res = ( prices[right] - prices[left] ) > res ? ( prices[right] - prices[left] ) : res;
            }
            right++;
        }
        return res;
    }
};
```

