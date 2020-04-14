# 77. Combinations

## Problem Description

Given two integers n and k, return all possible combinations of k numbers out of 1 ... n.

## Solutions

***
### 1. Backtracking

1. If the combination is complete-added to the output.

2. Iterate over all integers from first t to n.

    1. Add the integer i to the existing combination curr.

    2. Continue adding more integers to the combination: backtrack (i + 1, curr).

    3. Remove i from curr to achieve backtracking.

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
        def backtrack(first = 1, curr = []):
            # if the combination is done
            if len(curr) == k:  
                output.append(curr[:])
            for i in range(first, n + 1):
                # add i into the current combination
                curr.append(i)
                # use next integers to complete the combination
                backtrack(i + 1, curr)
                # backtrack
                curr.pop()
        
        output = []
        backtrack()
        return output
```

***
### 2. Dynamic Programming

There are two options for taking k from the remaining numbers: <br />
1. Take the current number, then take k-1 from the following number<br />
2. If you don't take this number, take k from the following numbers
When k is already 0, it means it can be added.<br />
If it is not possible to add, and the rest can not continue to add, then directly return<br />

```C
class Solution {
public:

    void Resort(int n,int nowIndex,int maxK,int k,vector<vector<int>> &res,vector<int> &tempVec)
    {
        if(k == 0)
        {
            res.push_back(tempVec);
            return;
        }

        if(k > n - nowIndex || nowIndex >= n || k < 0)
            return ;

        tempVec[maxK - k] = nowIndex + 1;
        Resort(n,nowIndex + 1,maxK,k - 1,res,tempVec);
        Resort(n,nowIndex + 1,maxK,k,res,tempVec);
    }

    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> tempVec;
        if(n < k)
            return res;
        for(int i = 0;i < k;++i)
            tempVec.push_back(i + 1);
        Resort(n,0,k,k,res,tempVec);
        return res;
    }
};
```

