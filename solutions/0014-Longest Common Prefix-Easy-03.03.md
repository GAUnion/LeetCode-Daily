# Longest Common Prefix

## Char by Char compare
If the length of the longest common prefix is k and the number of strings is n, the time complexity is at least O(n*k), because you have to check the first k characters for each string. So you start with 0 (the first character), and for each i, you check whether each string have length at least i, and check whether each string's ith character is the same. If either of the conditions fails, you can stop and return the first i characters. And remember to return '' if strs is an empty list.

```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if strs==[]:
            return ''
        for i in range(len(strs[0])):
            for str_ in strs:
                if i>=len(str_) or str_[i]!=strs[0][i]:
                    return strs[0][:i]
```



## Divide and conquer

Divide the array of strings into two parts, and then, divide each sub-array into two parts and do it until each sub-array has only one string. After that, start conquering. An array first gets the LCP (Longest Common Prefix) of its left sub-array, named LCP1 and then gets the LCP of its right sub-array, named LCP2. It returns the Longest Common Prefix of LCP1 and LCP2.

```python
class Solution:            
    def commonPrefixUtil(self, str1, str2):  
        for i in range(len(str1)):
            if i>=len(str2) or str2[i]!=str1[i]:
                return str1[:i]  

    def LCP_(self, arr, low, high):  
        if low == high: 
            return arr[low]  
        if high > low:  
            mid = low + (high - low) // 2
            str1 = self.LCP_(arr, low, mid)  
            str2 = self.LCP_(arr, mid + 1, high)  
            return self.commonPrefixUtil(str1, str2)
     
    def longestCommonPrefix(self, strs: List[str]) -> str:
    	return self.LCP_(strs, 0, len(strs) - 1) 


```

