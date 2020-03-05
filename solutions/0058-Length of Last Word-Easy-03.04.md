# Length of Last Word

### Method 1: traverse

This is the regular method. We can go through the string from right to left. Once we encounter the &quot; &quot;, we stop and realize that we have passed the last word.
However, we should pay attention to the last &quot; &quot; of the string. If the last character of the string is &quot; &quot;, the last word is the one in front of the last &quot; &quot;, rather than the one behind the last &quot; &quot;.
In Python, the rstrip() method returns a copy of the string with trailing characters removed.

```python
class Solution:
    def lengthOfLastWord(self, s):
        s = s.rstrip()
        if not s:
            return 0
        l = len(s)
        for i in range(l - 1, -1, -1):
            if s[i] == " ":
                return l - i - 1
        return l
```
This method is also easy to understand in other languages.

***
### Method 2: string split method
We can also take the advantage of Python and use the string split() Method. 
It returns a list of strings after breaking the given string by the specified separator.
Obviously, we would like to cut the string by &quot; &quot;. Then we can return the length of the last element directly.

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        s=s.split()
        if not s:
            return 0
        return len(s[-1])
```
In Java, the trim() method has the similar function as the split method.