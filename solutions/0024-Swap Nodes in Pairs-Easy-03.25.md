# Swap Nodes in Pairs

### Method 1: Recursive algorithm

We can swap the first two nodes and then put the rest of the list to *swapPairs* function. The termination condition is that the *head* or *head.next* is none. We can just return *head*. It is easy to get the code:

```python
class Solution(object):
    def swapPairs(self, head: ListNode) -> ListNode:
        if not head or not head.next: return head
        first_node, second_node = head, head.next
        first_node.next, second_node  = self.swapPairs(second_node.next), first_node
        return second_node
```

### Method 2: Iteration

Actually, this method is as the same as method 1 essentially. We have to traverse even and odd nodes separately, and then exchange the two nodes. The code is bellow:

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        pre = ListNode(0)
        pre.next = head
        node = pre
        while head and head.next:
            first_node, second_node = head, head.next
            prev = second_node
            first_node.next = second_node.next
            second_node.next = first_node
            pre, head = first_node, first_node.next
        return node.next
```