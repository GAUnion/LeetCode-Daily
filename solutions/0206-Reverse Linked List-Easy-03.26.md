# 206. Reverse Linked List

## Description

Reverse a singly linked list.

**Example:**

```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

**Follow up:**

A linked list can be reversed either iteratively or recursively. Could you implement both?

## Solution

### 1. Iterative I

First, we create a new node **pre** before the **head** to help this reversing process. Then, we keep inserting the latest node we meet (**head->next**) after the **pre** until the **head** becomes the last node.

```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head==NULL)  return head;
        ListNode* pre = new ListNode(0);
        pre->next = head;
        while(head->next){
            ListNode* temp = head->next->next;
            head->next->next = pre->next;
            pre->next = head->next;
            head->next = temp;
        }
        return pre->next;
    }
};
```

### 2. Iterative II

The second iterative solution is to reverse a pair of nodes each time from the beginning to the end.

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* newhead=NULL;
        ListNode* next;
        while(head){
            next = head->next;
            head->next = newhead;
            newhead = head;
            head = next;
        }
        return newhead;
    }
};
```

### 3. Recursive

Each time we call the recursive function, we reverse the node and the next one. And the last node's next should be set to **NULL**, so we simple set it each time.

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !(head -> next)) {
            return head;
        }
        ListNode* node = reverseList(head -> next);
        head -> next -> next = head;
        head -> next = NULL;
        return node;
    }
};
```

