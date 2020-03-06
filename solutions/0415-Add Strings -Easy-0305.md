# Add Strings

**Problem**: Given two non-negative integers `num1` and `num2` represented as string, return the sum of `num1` and `num2`. 

## Vertical Addition

This method is simulating vertical addition. Add bit by bit to calculate the current bit addition and whether a carry will occur.

Start from the end of each string. If the length of a string is not enough, the value will be 0. Calculate the carry digit. If the carry digit is not 0 in the end, then add ‘1’ in the front of the string.

```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        i = len(num1) - 1
        j = len(num2) - 1
        res = ''
        carry = 0
        while i >= 0 or j >= 0:
            if i >= 0:
                carry += ord(num1[i]) - ord('0')
            if j >= 0:
                carry += ord(num2[j]) - ord('0')
            res += chr(carry % 10 + ord('0'))
            carry //= 10
            i -= 1
            j -= 1
        if carry == 1:
            res += '1'
        return res[::-1]
```

If we do not use ord () and chr () functions to convert between characters and ASCII code, we can calculate each character directly. This is a kind of two pointers algorithm.

 ````python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        i = len(num1) - 1
        j = len(num2) - 1
        res = ''
        carry = 0
        while i >= 0 or j >= 0:
            n1 = int(num1[i]) if i >= 0 else 0
            n2 = int(num2[j]) if j >= 0 else 0
            tmp = n1 + n2 + carry
            carry = tmp // 10
            res = str(tmp % 10) + res
            i -= 1
            j -= 1
        return "1" + res if carry else res
 ````

