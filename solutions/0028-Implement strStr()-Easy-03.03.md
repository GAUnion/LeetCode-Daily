# Implement strStr()
**Problem** Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

### Method 1: Substring
This method solves it by violence, and the time complexity is *O(MN)*
The method has two loops.
In the first loop, we extract every substring in the text *haystack* which match the size of the pattern *needle*;
In the inner loop, we compare each substring with the pattern character by character, once a mismatched character pair emerge, we break the inner loop and continue to compare the next substring.
```python
class Solution(object):
    def strStr(self, haystack, needle):
        if len(needle)==0:
            return 0
        for i in range(len(haystack)-len(needle)+1):
            if haystack[i:i+len(needle)] == needle:
                return i
        return -1
```
Above is the implementation by python, the inner loop is omitted as a result of the easy access of substring in python. If coded by *C*, the inner loop should be compared char by char and break in time to avoid redundant comparison.
***
### Mehod 2: Knuth-Morris-Pratt (KMP) Matcher
KMP is an efficient string pattern matching algorithm, the time complexity is linear.
The algorithm has two key procedure:
- Calculate the prefix array according to the maxium length of the common element in both the prefix set and the suffix set of each substring (the maxium length is smaller than the length of the substring)
- Move the pattern as a window across the target text according to the prefix table

Here is a vivid presentaion of how KMP works:
[video](https://www.youtube.com/watch?v=dgPabAsTFa8)
First we need to calculate the prefix array:
take "ABCDABD" as an example
- "A": both the prefix set and suffix set set are []，the maxium length of commom element is 0；
- "AB": the prefix set is[A]，the suffix set is[B]，the maxium length of commom element is 0；
- "ABC": the prefix set is[A, AB]，the suffix set is[BC, C]，the maxium length of commom element is 0；
- "ABCD": the prefix set is[A, AB, ABC]，the suffix set is[BCD, CD, D]，the maxium length of commom element is 0；
- "ABCDA": the prefix set is[A, AB, ABC, ABCD]，the suffix set is[BCDA, CDA, DA, A]，the common element is "A"，the length is 1；
- "ABCDAB": the prefix set is[A, AB, ABC, ABCD, ABCDA]，the suffix set is[BCDAB, CDAB, DAB, AB, B]，the common element is "AB"，the length is 2；
- "ABCDABD": the prefix set is[A, AB, ABC, ABCD, ABCDA, ABCDAB]，the suffix set is[BCDABD, CDABD, DABD, ABD, BD, D]，the maxium length of commom element is 0。

```python
def prefix_table(pattern): 
        prefix = [0] * (len(pattern)+1) #      A  B  C  D  A  B  D
        prefix[0] = -1                  # [-1, 0, 0, 0, 0, 1, 2, 0]
        i, j = 0, -1
        while (i < len(p)):
            if (j == -1 or p[i] == p[j]):
                i += 1
                j += 1 
                prefix[i] = j
            else:
                j = prefix[j]
        return prefix
```
Each element of the prefix array is calculated based on its former item, if the extended suffix is the same as the extended prefix, then the prefix length will accumulate 1. If you feel confused about it, try the linked video appended above or find some other reference materials.

Second we move the needle and search for the matching pattern according to the prefix table caculated before.
```python
def kmp(text, pattern, prefix):
        i, j = 0, 0
        while (i < len(text) and j < len(pattern)):
            if (j == -1 or text[i] == pattern[j]):
                i += 1
                j += 1
            else:
                j = prefix[j]
        if j == len(pattern):
            return i - j
        else:
            return -1
```
To sum
```python
return kmp(haystack, needle, prefix_table(needle))
```
