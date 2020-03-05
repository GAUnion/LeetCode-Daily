# First Unique Character in a String
**Problem** Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return **-1**.

## Dictionary --Hash Table
In order to find the first non-repeating charater, a dictionary can be built to store each charater in the string and its number of occurrences, then traversing the string again, we can get the final result, which is the first character corresponding to the value **1** in the dictionary.
    
```python
class Solution:
def firstUniqChar(self, s: str) -> int:
    dic = {}
    for char in s:
        dic[char] = s.count(char)
    for value,key in enumerate(s):
        if dic[]:
            return i
    return -1
```

But using this method will spend too much time or even exceed the time limit. To optimize, we can use the function set() to filter the string to avoid repeated character when traversing.

```python
class Solution:
def firstUniqChar(self, s: str) -> int:
    dic = {char: s.count(char) for char in set(s)}
    for  value,key in enumerate(s):
        if dic[key] == 1:
            return value
    return -1
```



## Special optimization in this problem

Noticing that in this question, the string contain only lowercase letters, so the keys in the dictionary can only be **a** to **z**. Also, when judging whether a character is unique, we can use find() and rfind() to judge whether the first occurrence of one character is the last.

```python
class Solution(object):

def firstUniqChar(self, s: str) -> int:
    min_unique_char_index = len(s)

    for char in "abcdefghijklmnopqrstuvwxyz":
        i = s.find(char)
        if i != -1 and i == s.rfind(char):
            min_unique_char_index = min(min_unique_char_index, i)

    return min_unique_char_index if min_unique_char_index != len(s) else -1
```

## One pass method

We use two data structure to solve this problem in one pass. The set named "occurence" to record which characters are already occured. If the character occured twice, the array would marked it as -1, or record the pos of first occurence of character. 

```java
class Solution {
    public int firstUniqChar(String s) {
        int[] pos = new int[26]; // the pos of first occurence of char, if repeated, not existed, -1;
        Arrays.fill(pos, -1);
        Set<Character> occurence = new HashSet<>(); 
        
        for(int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if(!occurence.contains(ch)) {
                occurence.add(ch);
                pos[ch - 'a'] = i;
            } else {
                pos[ch - 'a'] = -1;
            }
        }
        
        int minIndex = Integer.MAX_VALUE;
        for(int i = 0; i < 26; i++) {
            if(pos[i] != -1) minIndex = Math.min(minIndex, pos[i]);
        }
        
        return minIndex == Integer.MAX_VALUE ? -1 : minIndex;
    }
}
```

The code is written in Java.