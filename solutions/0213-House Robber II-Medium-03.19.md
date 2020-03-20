# 213. House Robber II

### Problem

------

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight **without alerting the police**.

### Solution : Dynamic Programming

------

This problem is a follow-up to **House Robber** and assume now we already know how to rob the house in a row. The only difference is that now all the house are arranged in a circle, which means that House[1] and House[n] are adjacent and the theif cannot rob these two together. Therefore, the problem becomes to rob either House[1]-House[n-1] or House[2]-House[n], depending on which choice offers more money. Now the problem has degenerated to the House Robber, which is already been solved.

```Java
class Solution {
    public int rob(int[] nums) {
        if (nums.length == 1) {
            return nums[0];
        }
        return Math.max(robAtRange(nums, 0, nums.length - 1), robAtRange(nums, 1, nums.length));
    }
    public int robAtRange(int[] nums, int begin, int end) {
        int include = 0, exclude = 0;
        for (int j = begin; j < end; i++) {
            int preInclude = include, preExclude = exclude;
            exclude = Math.max(preInclude, preExclude);
            include = preInclude + nums[j];
        }
        return Math.max(include, exclude);
    }
}
```

Time Complexity : O(N)

Space Complexity : O(1)