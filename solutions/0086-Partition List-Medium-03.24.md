# Partition List
## Problem Description

Given a linked list and a value *x*, partition it such that all nodes less than *x* come before nodes greater than or equal to *x*.

You should preserve the original relative order of the nodes in each of the two partitions.

**Example:**

```
Input: head = 1->4->3->2->5->2, x = 3
Output: 1->2->2->4->3->5
```



## Solution

### Two Pointers

I have to say it took me a while to understand what the meaning of this problem is..

But anyway, we need two pointers, in my case, "less" - to record the node/value of the ones which less than x, and another one "greater" to record equal and greater than x. Record its initial position and iteratively add element to these two pointers. And finally link these two pointers. It should be a "Easy" problem if one have basic information on linked list.

And one more thing, you can directly assign head to less->next, for example, not create a new "listnode", but need to break the link at the end.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode *less = new ListNode(0), *lessHead = less;
        ListNode *great = new ListNode(0), *greatHead = great;
        while(head != NULL) {
            if(head->val < x) {
                less->next = new ListNode(head->val);
                less = less->next;
            }
            else {
                great->next = new ListNode(head->val);
                great = great->next;
            }
            head = head->next;
        }
        less->next = greatHead->next;
        return lessHead->next;
    }
};
```

