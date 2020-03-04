# Longest Common Prefix

## Method1
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



