### [LeetCode 53 Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

**Solution 1**	Dynamic Programming

The variable dp records the maximum sum of the first i elements, which is the addition of the ith element or only the reservation of the ith element.

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    int dp = nums[0], res = nums[0];
    for (int i = 1; i < nums.length; i++) {
        dp = Math.max(dp + nums[i], nums[i]);
        //dp = nums[i] + (dp > 0 ? dp : 0); //this line could also find the maximum
        res = Math.max(res, dp);
    }
    return res;
}
```

**Solution 2** 	Divide and Conquer 

In the problem, the following up question is: if you have figured out the O(*n*) solution, try coding another solution using the divide and conquer approach, which is more subtle.

By finding out the middle element, there are three conditions related to calculating the maximum:

1. left maximum
2. right maximum
3. maximum including the middle element

```java
public int maxSubArray(int[] nums) {
    if (nums == null || nums.length == 0) return 0;
    return helper(nums, 0, nums.length - 1);
}
private int helper(int[] nums, int start, int end) {
    if (start == end) return nums[start];
    if (start > end) return Integer.MIN_VALUE;
    int middle = (start + end) / 2; 
    int left_max = 0, right_max = 0;
    int suml = 0, sumr = 0, ml = 0, mr = 0;
    left_max = helper(nums, start, middle - 1);
    right_max = helper(nums, middle + 1, end);
    for (int i = middle - 1; i >= start; i--) {
        suml += nums[i];
        ml = Math.max(ml, suml);
    }
    for (int j = middle + 1; j <= end; j++) {
        sumr += nums[j];
        mr = Math.max(mr, sumr);
    }
    return Math.max(Math.max(left_max, right_max), ml + nums[middle] + mr);
}
```



