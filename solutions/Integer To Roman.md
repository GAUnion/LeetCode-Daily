# Integer To Roman

### Method: Greedy Algorithm

It is typical problem of greedy algorithm. We must use least characters to represent integers, so we use the biggest character as many as possible. Take '22' as an example, we should use ten twice and then use one twice. Other combinations are not legal.

First, we can set a rule to connect integer and Roman characters, like:

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        rule = (
            ('M', 1000),
            ('CM', 900), ('D', 500), ('CD', 400), ('C', 100),
            ('XC', 90), ('L', 50), ('XL', 40), ('X', 10),
            ('IX', 9), ('V', 5), ('IV', 4), ('I', 1),
        )
```


Then we use Greedy Algorithm to form the Roman version from integer:


```python

        res = ''
        for roman, integer in rule:
            if num >= integer:
                res += roman * (num // integer)
                num %= integer
        return res
```


Finally we can get the right answer.

***