# 395. Longest Substring with At Least K Repeating Characters

## Problem Description

Find the length of the longest substring ***T\*** of a given string (consists of lowercase letters only) such that every character in ***T\*** appears no less than *k* times.



## Solutions

### 1. Recursion

The idea of this solution is to look for characters which appear less than *k* times and then recursively search the substrings between these characters for longest length. So in this way, we can use an array to record the frequency of each cahracter. Then divide the string. Remember to check the corner case. 

```
class Solution {
    
public:
    int substring(const string& s, int k,int start, int end)
    {
        if(end-start < k)
            return 0;
        
        int max_result = 0;
        
        int count[26] = {0};
        
        int tmp = start;
        
        for(int i = start; i<end; i++)
            count[s[i]-'a']++;
        
        for(int i = start; i < end; i++)
        {
            if(count[s[i]-'a']<k && count[s[i]-'a']!=0)
            {
                max_result = max(max_result, substring(s,k,tmp,i));
                tmp = i+1;      
            }
        }
        
        if(tmp == start)
            return end - start;
        
        return max(max_result,substring(s,k,tmp,end));
    }
    
    int longestSubstring(string s, int k) {
        return substring(s,k,0,s.length());
    }
};
```

### 2. Iteration

This solution is to count the number of characters which appears no less than *k* times.(count) Then iteratively find the max length of a sliding window which contains i unique characters (1 <=i <= count) satisfing the condition above.

```
class Solution {
    
public:
    int get(const string& s, const int chars, const int k) {
        int size = s.size(), i = 0, j = 0, charCount = 0, kCount = 0, len = 0, v;
        vector<int> m(26, 0);
        while(j < size) {
            v = ++m[s[j++]-'a'];
            if(v == 1) ++charCount;
            if(v == k) ++kCount;
            while(charCount > chars) {
                v = --m[s[i++]-'a'];
                if(v == 0) --charCount;
                else if(v == k-1) --kCount;
            }
            if(charCount == chars && kCount == chars) len = max(len, j-i);
        }
        return len;
    }
    
    int longestSubstring(string s, int k) {
        vector<int> m(26, 0);
        int count = 0, len = 0;
        for(const char c: s)
            if(++m[c-'a'] == k) ++count;
        for(int i = 1; i <= count; ++i) len = max(len, get(s, i, k));
        return len;
    }
};
```



