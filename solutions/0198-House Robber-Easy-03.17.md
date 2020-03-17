# House Robber

### Method: Dynamic Programming

It is not possible to enumerate every combination of houses. We should observe the relation of the current money and pre money.

Let's think about the robbing strategy when you meet the next house. I have two options: 
1. Leave this house be because the former one is too valuable.
2. Rob this house and give up the former one. And the money of the houses before the former one remains.

Assuming next is the maximum money with the next house, current is the current money and pre is the pre money. When you arrive at the next house, you may consider the strategies above. So here is state transition equation:
$$
next = max(current, pre+theMoneyOfTheNextHouse)
$$
Now we can get the code:

```python
class Solution:
    def rob(self, nums: List[int]) -> int:
        cur, pre = 0, 0
        for n in nums: cur, pre = max(cur, pre + n), cur
        return cur
```

***

