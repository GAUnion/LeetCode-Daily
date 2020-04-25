# Majority Element

### Method: Boyer-Moore Voting Algorithm 

We maintain a candidate mode candidate and its number of occurrences count. Initially, candidate can be any value and count is 0.

We traverse all elements in the array nums. For each element x, before judging x, if the value of count is 0, we first give the value of X to candidate, and then we judge X:

​		If x is equal to candidate, the counter count is incremented by 1.

​		If x is not equal to candidate, the counter count is reduced by 1.

After traversal, candidate is the mode of the whole array.

```python
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        if len(nums) == 1: return nums[0]
        voter = (nums[0], 1)
        for i in nums[1:]:
            if i != voter[0]:
                if voter[1] == 1: voter = (i, 1)
                else: voter = (voter[0], voter[1]-1)
            else: voter = (voter[0], voter[1]+1)
        return voter[0]
```
