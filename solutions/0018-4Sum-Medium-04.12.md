# Four-Sum

### Description 

Given an array `nums` of *n* integers and an integer `target`, are there elements *a*, *b*, *c*, and *d* in `nums` such that *a* + *b* + *c* + *d* = `target`? Find all unique quadruplets in the array which gives the sum of `target`.

**Note:**

The solution set must not contain duplicate quadruplets.

**Example:**

```
Given array nums = [1, 0, -1, 0, -2, 2], and target = 0.

A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
```

### Solution 1 - Two Sum + Recursion

It is actually a solution to solve K-Sum problem. 

**General Idea** (Credit to [this discussion](https://leetcode.com/problems/4sum/discuss/8609/My-solution-generalized-for-kSums-in-JAVA))

If you have already read and implement the 3sum and 4sum by using the sorting approach: reduce them into 2sum at the end, you might already got the feeling that, all k-Sum problem can be divided into two problems:

1. 2sum Problem
2. Reduce K sum problem to K – 1 sum Problem

**My C++ Implementation** (16ms, faster than 85.35%)

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> results; vector<int> result;
        fourSum(nums, 0, nums.size()-1, target, 4, result, results);
        return results;
    }
    void fourSum(vector<int>& nums, int l, int r, int target, int N, vector<int> result, vector<vector<int>> &results){
        if (r-l+1 < N || N < 2 || target<nums[l]*N || target>nums[r]*N) return;
        if (N == 2){
            while(l < r){
                int s = (nums[l] + nums[r]);
                if (s == target){
                    result.push_back(nums[l]);
                    result.push_back(nums[r]);
                    results.push_back(result);
                    result.pop_back(); result.pop_back();
                    l += 1; r-=1;
                    // Note here: avoid repeating
                    while (l<r && nums[l]==nums[l-1]) l+=1;
                    while (l<r && nums[r]==nums[r+1]) r-=1;
                }
                else if (s > target) r-=1;
                else l += 1;
            }
        }
        else {
            for (int i = l; i<r; i++) {
                // Note here: avoid repeating
                if (i > l && nums[i] == nums[i-1]) continue;
                result.push_back(nums[i]);
                fourSum(nums, i+1, r, target - nums[i], N-1, result, results);
                result.pop_back();
            }
        }
    }
};
```

I note in this code at some place to tell you guys to be careful about the repetitions of results.

**Python Implementation** (credit to [this discussion](https://leetcode.com/problems/4sum/discuss/8545/Python-140ms-beats-100-and-works-for-N-sum-(Ngreater2)))

Passing Pointer, not sliced list

```python
def fourSum(self, nums, target):
    def findNsum(l, r, target, N, result, results):
        if r-l+1 < N or N < 2 or target < nums[l]*N or target > nums[r]*N:  # early termination
            return
        if N == 2: # two pointers solve sorted 2-sum problem
            while l < r:
                s = nums[l] + nums[r]
                if s == target:
                    results.append(result + [nums[l], nums[r]])
                    l += 1
                    while l < r and nums[l] == nums[l-1]:
                        l += 1
                elif s < target:
                    l += 1
                else:
                    r -= 1
        else: # recursively reduce N
            for i in range(l, r+1):
                if i == l or (i > l and nums[i-1] != nums[i]):
                    findNsum(i+1, r, target-nums[i], N-1, result+[nums[i]], results)

    nums.sort()
    results = []
    findNsum(0, len(nums)-1, target, 4, [], results)
    return results
```

### Solution 2 - 3Sum-Based Solution

Credit to [this post](https://leetcode.com/problems/4sum/discuss/8714/4Sum-C%2B%2B-solution-with-explanation-and-comparison-with-3Sum-problem.-Easy-to-understand.)

The key idea is to downgrade the problem to a `2Sum` problem eventually. And the same algorithm can be expand to `NSum` problem.

After you had a look at my explanation of `3Sum`, the code below will be extremely easy to understand

```C++
class Solution {
public:
    vector<vector<int> > fourSum(vector<int> &num, int target) {
    
        vector<vector<int> > res;
    
        if (num.empty())
            return res;
    
        std::sort(num.begin(),num.end());
    
        for (int i = 0; i < num.size(); i++) {
        
            int target_3 = target - num[i];
        
            for (int j = i + 1; j < num.size(); j++) {
            
                int target_2 = target_3 - num[j];
            
                int front = j + 1;
                int back = num.size() - 1;
            
                while(front < back) {
                
                    int two_sum = num[front] + num[back];
                
                    if (two_sum < target_2) front++;
                
                    else if (two_sum > target_2) back--;
                
                    else {
                    
                        vector<int> quadruplet(4, 0);
                        quadruplet[0] = num[i];
                        quadruplet[1] = num[j];
                        quadruplet[2] = num[front];
                        quadruplet[3] = num[back];
                        res.push_back(quadruplet);
                    
                        // Processing the duplicates of number 3
                        while (front < back && num[front] == quadruplet[2]) ++front;
                    
                        // Processing the duplicates of number 4
                        while (front < back && num[back] == quadruplet[3]) --back;
                
                    }
                }
                
                // Processing the duplicates of number 2
                while(j + 1 < num.size() && num[j + 1] == num[j]) ++j;
            }
        
            // Processing the duplicates of number 1
            while (i + 1 < num.size() && num[i + 1] == num[i]) ++i;
        
        }
    
        return res;
    
    }
};
```

### Complexity Discussion

Thanks to [this discussion](https://leetcode.com/problems/4sum/discuss/8565/Lower-bound-n3), we got a lower bound proof for the algorithm for this problem

> Some people say their solutions are O(n2 log n) or even O(n2), but...

Consider cases where nums is the n numbers from 1 to n.
=> There are Θ(n4) different quadruplets (nC4, to be exact, so about n4 / 24).
=> There are Θ(n) possible sums (from 1+2+3+4 to (n-3)+(n-2)+(n-1)+n, so about 4n sums).
=> At least one sum must have Ω(n3) different quadruplets.
=> For that sum, we must generate those Ω(n3) quadruplets.
=> **For these cases we have to do Ω(n3) work.**
=> **O(n2 log n) or even O(n2) are impossible.**