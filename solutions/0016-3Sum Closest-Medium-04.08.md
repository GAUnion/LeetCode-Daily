# 16.3Sum Closest
**Problem**: Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.


##Double pointer
In this problem, a simple solution to it is traversing the array to find every 3sum, but the time complexity will be o(n^3). To optimize, we can use sort and double pointer.

The idea of this method is to let the three nums be fixed value (traversing the array), start pointer and end pointer. For each fixed value, initial start pointer and end pointer is the next element and the last element. 

The next step is to add this three nums, compare with the former closest value, and update closest value. If the new value is bigger than target, since the array is sorted, we can reduce end pointer by 1 to make the three sum smaller, similarly, add start pointer by 1 if the new value is smaller than target. In the end, simply return the closest value after traversing. In this method, the time complexity is o(nlogn)(sort) + o(n^2)(traverse).

```python
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums = sorted(nums)
        result = nums[0] + nums[1] + nums[2]
        for i in range(len(nums)-2):
            start = i+1
            end = len(nums)-1
            while start < end:
                currentsum = nums[i] + nums[start] + nums[end]
                if (abs(currentsum-target) < abs(result - target)):
                    result = currentsum
                if (result == target): 
                    return result
                if (result < target):
                    start += 1
                if (result > target):
                    end -= 1
        return result
```



