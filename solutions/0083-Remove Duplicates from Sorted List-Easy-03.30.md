# Remove Duplicates from Sorted List

### Problem
Given a sorted linked list, delete all duplicates such that each element appear only once.


### Solution

This is a typical and easy linked list problem, the key to this problem is the use of pointer. We can let the pointer point to the next unique element while head == head.next. Thus the program will be as follow:

```python
class Solution:
    def deleteDuplicates(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return head
        node = head
        while node != None and node.next != None:
            if node.next.val == node.val:
                node.next = node.next.next
            else:
                node = node.next
        return head
```