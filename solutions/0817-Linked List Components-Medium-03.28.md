# Linked List Components

### Method: Traverse List

Scan the whole list started with head. Use a variable flag to denote whether the last node is in G. When meeting a value not in G, set flag to 0. If flag is 0 and meet a value in G, add 1 to the number of components.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def numComponents(self, head: ListNode, G: List[int]) -> int:
        if head==None or G==None or G==[]:
            return 0
        flag=0
        res=0
        s=set(G)
        while(True):
            if head.val in s and flag==0:
                flag=1
                res+=1
            elif not (head.val in s):
                flag=0
            if head.next==None:
                return res
            head=head.next
        
```



