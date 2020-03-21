# 132. Palindrome Partitioning II

## Problem Description

Given a string *s*, partition *s* such that every substring of the partition is a palindrome.

Return the minimum cuts needed for a palindrome partitioning of *s*



## Solution

### Dynamic Programming

First, set the value in the dp array from 0 to n. This is the max cut each portion can get. The palindrome can be of odd length or even length. So we deal with each of them separately. Check from the current point, use two pointers i,j to extend to the start and end. If the value of the pointers are the same, update the value in dp[j+1], else, break from the loop. 

Remember to subtract 1 from the final dp[n], because the final end is naturally already a cut.



```
    int minCut(string s) {
        int n = s.size();
        vector<int> dp(n + 1);
        iota(begin(dp), end(dp), 0);
        for (int m = 0; m < n; ++m) {
            
            // even length palindrome with each i, i+1 till as pivot of palindrome
            // check how bigger a even length palindrome is possible with 
            // (m,m+1) as starting of the pivot. update the end of the palindrome index dp entry as 
            // 1 + dp[i], where is ith location palindrome starts can be consumed in 1 cut.
            // hence 1 + dp[i], minimumm cuts till ith location
            for (int i = m, j = m + 1; i >= 0 && j < n ; --i, ++j) {
                if (s[i] != s[j])
                    break;
                dp[j + 1] = min(dp[j + 1], 1 + dp[i]);
            }
            
            // odd length palindrome with each i as pivot
            // check how bigger a odd length palindrome is possible with m as starting of the pivot.
            for (int i = m, j = m; i >= 0 && j < n; --i, ++j) {
                if (s[i] != s[j])
                    break;
                dp[j + 1] = min(dp[j + 1], 1 + dp[i]);
            }
        }
        return dp[n] - 1;
    }
```

