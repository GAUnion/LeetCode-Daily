# 303.Range Sum Query-Immutable

## Problem

Given an integer array *nums*, find the sum of the elements between indices *i* and *j* (*i* ≤ *j*), inclusive.

**Example:**

```
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

## Solution

Create an array `sum` that stores the accumulated sum for `nums` such that sum[i+1] = nums[0] + ... + nums[i].  Through dynamic programming, sum[i+1] = sum[i] + nums[i]. Then we can calculate sumRange using: sumRange(i, j) = sum[j+1] - sum[i].

**Code in C++**

````c++
class NumArray {
     vector<int> sum;
public:
    NumArray(vector<int>& nums) {
        sum = vector<int>(nums.size() + 1);
        sum[0] = 0;
        for(int i = 0; i < nums.size(); i++){
            sum[i+1] = sum[i] + nums[i];
        }
    }    
    int sumRange(int i, int j) {
        return sum[j+1] - sum[i];
    }
};
/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
````

Time complexity: O(N)
Space complexity: O(N)



**Code in python**

````python
class NumArray:
    def __init__(self, nums: List[int]):
        self.sum = [0] * (len(nums) + 1)
        self.sum[0] = 0
        for i in range(0,len(nums)):
            self.sum[i+1] = self.sum[i] + nums[i] 

    def sumRange(self, i: int, j: int) -> int:
        return self.sum[j+1] - self.sum[i]

# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
````

