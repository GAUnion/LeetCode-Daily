# Add Two Numbers II

### Problem

You are given two non-empty linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.

### Solution

We use two stacks to store two linked lists. The top of the stack is the lower order of the number, and the top of the stack is the higher order of the number. Then the two stack elements are added correspondingly. Pay attention to the carry-over situation.

```
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        n1 = []
        n2 = []
        while l1:
            n1.append(l1.val)
            l1 = l1.next
        while l2:
            n2.append(l2.val)
            l2 = l2.next
        count = 0
        result = []
        while n1 or n2 or count:
            a = n1.pop() if n1 else 0
            b = n2.pop() if n2 else 0        
            sum = a + b + count       
            if sum<10:
                result.append(sum)
                count = 0
            else:
                result.append(sum-10)
                count = 1
        res = ListNode(0)
        r = res
        for i in result[::-1]:
            r.next = ListNode(i)
            r = r.next
        return res.next
```

