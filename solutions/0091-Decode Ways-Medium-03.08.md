# 91. Decode Ways

I actually failed this problem...

## 1.Basic Recursion

Time Complexity: O(2^n)

Space Complexity: O(1)

Some tips when coding:

1. At first ignore the situation when '0' is at the head of sub-string, which indicates the sub-string could not be encoded;
2. pass the position rather than the whole sub-string, saving both time and space

```c++
class Solution {
public:
    int numDecodings(string s) {
        return s.empty() ? 0 : decode(0, s);
    }
    int decode(int pos, string &s) {
        int n = s.size();
        if(pos == n) return 1;
        if(s[pos] == '0') return 0;
        int res = decode(pos+1, s);
        if(pos < s.size()-1 && (s[pos] == '1' || (s[pos] == '2' && s[pos+1] < '7')))
            res += decode(pos+2, s);
        return res;
    }
};
```

## 2. Recursion with memoization

We could notice the result at each position is fixed, but it is recursively computed, so we could save the result after the first time we got the result. And the process is called "Memoization(记忆化)"

Time Complexity: O(n)

Space Complexity: O(n)

```C++
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> mem(n+1, -1);
        //Each result would be no less than 0, so we could set the initial value as -1
        //also noticed we allocate n+1 but not n positions, due to we need the position when pos == s.size(), like before
        mem[n] = 1;
        //Set the value of last pos, no need to calculate
        return s.empty() ? 0 : decode(0, s, mem);
    }
    
    int decode(int pos, string &s,vector<int> &mem) {
        if(mem[pos] > -1) return mem[pos]; 
        //if traversed, directly return the value
        if(s[pos] == '0') return 0;
        int res = decode(pos+1, s, mem);
        if(pos < s.size()-1 && (s[pos] == '1' || (s[pos] == '2' && s[pos+1] < '7')))
            res += decode(pos+2, s, mem);
        return mem[pos] = res;
    }
};
```

## 3. DP

We could notice the traverse before is one direction, so we could reversely re-code it as dynamic programming.

Same time and space as before, O(n) and O(n).

```c++
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> dp(n+1, -1);
        dp[n] = 1;
        for(int i = n-1; i >= 0; i--) {
            dp[i] = s[i] == '0' ? 0 : dp[i+1];
            //notice here is a "return" before, so we need to use a condition clause here
            if(i < n-1 && (s[i] == '1' || (s[i] == '2' && s[i+1] < '7')))
                dp[i] += dp[i+2];
        }
        return s.empty() ? 0 : dp[0];
    }
};
```

## 4.DP with O(1) space

Noticing each loop would only use the values of i+1 and i+2, thus we could remove vector and save them as numbers

```c++
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        int cur = 0, p = 1, pp = 0;
        for(int i = n-1; i >= 0; i--) {
            cur = s[i] == '0' ? 0 : p;
            if(i < n-1 && (s[i] == '1' || (s[i] == '2' && s[i+1] < '7')))
                cur += pp;
            pp = p;
            p = cur;
        }
        return s.empty() ? 0 : cur;
    }
};
```

