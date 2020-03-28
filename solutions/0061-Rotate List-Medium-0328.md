# 61. Rotate List

## Problem Description

Given a linked list, rotate the list to the right by k places, where k is non-negative.

## Solutions

***
### 1. Linked list

First, find the length of the linked list. If k takes the remainder of size, then no rotation is required, and head is returned directly; otherwise, the linked list is connected end to end to form a circular linked list. Move the size k%size bit to the right. At this time, tail moves the size k%size bit to reach the predecessor node of the new head node. We just need to save the new head node and disconnect the linked list at the same time.

```C++
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head||k==0)return head;
        ListNode *tail=head;
        int size=1;
        while(tail->next){
            size++;
            tail=tail->next;
        }
        if(k%size==0)return head;
        tail->next=head;
        int m=size-k%size;
        while(m--)tail=tail->next;
        ListNode *res=tail->next;
        tail->next=nullptr;
        return res;
    }
};
```

***
### 2. Fast and slow pointer

a. Find the length of the linked list len <br />
b. k = k% len margin is the distance we want to move to the right.<br />
c. Find the k-th position from the bottom. You can use the double pointer method.<br />
d. Record the next node of the slow pointer. This is the last node to be returned.<br />
e. Record the position of next, which is the head node returned.<br />
f. Disconnect, the tail node of the next paragraph points to the head node of the first paragraph.


```c++
public ListNode rotateRight(ListNode head, int k) {

        if (head == null || k == 0) {
            return head;
        }
        ListNode tmp = head;
        int len = 0;
        while (tmp != null) {
            tmp = tmp.next;
            len++;
        }
        k = k % len;
        if (k == 0) {
            return head;
        }
        ListNode node = head;
        tmp = head;
        while (k > 0) {
            k--;
            tmp = tmp.next;
        }
        while (tmp.next != null) {
            head = head.next;
            tmp = tmp.next;
        }
        ListNode res = head.next;
        head.next = null;
        tmp.next = node;
        return res;
    }
```

