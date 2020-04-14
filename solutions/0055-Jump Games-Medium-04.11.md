# Keys and Rooms
## Problem Description

Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Determine if you are able to reach the last index.

**Example 1:**

```
Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```

**Example 2:**

```
Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum
             jump length is 0, which makes it impossible to reach the last index.
```

## Solution

### Backtracking/Brute-force

Maybe I've done too many graph problems these days, the first possible solution coming to my mind is BFS and use a queue to store the possible index to visit. After TLE, I realize I could change it to DFS to reach further for each loop, but it still results in TLE.

Although I called it "search", the proper name should be backtracking. We iteratively go to each possible index. For each index, there would be two status, passed or not passed, so there would be at most 2^n different routes, and the time complexity would be O(2^n). Space complexity would be O(n) for recursion memory stack.

```c++
class Solution {
public:
    bool jumpHelper(vector<int>& nums, int pos) {
        int siz = nums.size()-1;
        if(pos == siz)
            return true;
        int maxLength = min(siz, pos+nums[pos]);
        //A little optimization here to reversely iterate, try to reach further more quick
        for(int i = maxLength; i > pos; i--)
            if(jumpHelper(nums, i)) return true;
        return false;
    }
    bool canJump(vector<int>& nums) {
        return jumpHelper(nums, 0);
    }
};
```



### Top-down DP

We could find if one index could reach the final index is settled, but we recursively visited it and calculated several times. Thus we could add memorization to save the status of each index. If it is visited, we could directly use it without calculating again. Here we use 1 to present the index could reach the final index, -1 to present cannot, and 0 to present haven't reach yet. We initialized all value to 0, and set the final one as 1, of course it can reach itself, and then start recursion.

For this method, the time complexity would be O(n^2), since for each index, one could at most visited n index. And the space complexity would be O(n). But unfortunately, the result is still TLE.

```c++
class Solution {
public:
    bool jumpHelper(vector<int>& nums, vector<int>& status, int pos) {
        if(status[pos])
            return status[pos] > 0 ? true : false;
        int maxlen = min(int(nums.size()-1), pos + nums[pos]);
        for(int i = maxlen; i > pos; i--) {
            if(jumpHelper(nums, status, i)) {
                status[pos] = true;
                return true;
            }
        }
        status[pos] = -1;
        return false;
    }
    bool canJump(vector<int>& nums) {
        vector<int> status(nums.size(), 0);
        //-1 for nodes could not reach final, 1 for could, 0 for unknown
        status[nums.size()-1] = 1;
        return jumpHelper(nums, status, 0);
    }
};
```

### Bottom-up DP

Top-down DP is easy to find, and after that, we should change that to bottom-up DP, by find the end status and flip it over. This step would achieving better performance by avoiding some caching of recursion. What's more, it provide a bridge for us to further simplify DP.

```c++
class Solution {
public:
    bool canJump(vector<int>& nums) {
        vector<int> status(nums.size(), 0);
        //-1 for nodes could not reach final, 1 for could, 0 for unknown
        int len = nums.size();
        status[len-1] = 1;
        for(int pos = len-2; pos >= 0; pos--) {
            int maxlen = min(int(nums.size()-1), pos + nums[pos]);
            for(int i = maxlen; i > pos; i--) {
                if(status[i] == 1) {
                    status[pos] = 1;
                    break;
                }
            }
        }
        return status[0];
    }
};
```

### Greedy

Following by the previous step, we could find we don't need all status of index, we only need the one at the most left. If that one could be reach, then the current index would also could reach. Thus, we could change the list into one variable "lastpos". We iterate from right to left, if current index could reach lastpos, lastpos could become current position. Complexity would be O(n) and O(1).

```c++
class Solution {	//8ms
public:
    bool canJump(vector<int>& nums) {
        int lastpos = nums.size()-1;
        for(int i = nums.size()-1; i >= 0; i--)
            if(i + nums[i] >= lastpos)
                lastpos = i;
        return lastpos == 0;
    }
};
```

Here is another more intuitive one. We iterate from left to right, until the most further one we could make, and see if we could make it to the final one.

```c++
class Solution {	//4ms
public:
    bool canJump(vector<int>& nums) {
        int maxpos = 0;
        for(int i = 0; i <= maxpos; i++) {
            maxpos = max(maxpos, i+nums[i]);
            if(maxpos >= nums.size()-1) return true;
        }
        return false;
    }
};
```

This is my not so strict greedy. For each step, jump as much as we could, until we find we could not make it to the final index. Since we could reach all other step before the furthest one, go back to see if we could go further. Reaching -1 means we tried every method to go further but failed. Worst time complexity is O(n^2)

```c++
class Solution {	//8ms
public:
    bool canJump(vector<int>& nums) {
        if(nums.empty()) return true;
        int len = nums.size();
        int cur = 0, max = 0;
        while(cur < len-1 && cur != -1) {
            if(cur+nums[cur] > max) {
                cur += nums[cur];
                max = cur;
            }
            else
                cur--;
        }
        return max>=len-1;
    }
};
```



This could be a good interview problem.