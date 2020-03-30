## Problem
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

Example:

Given this linked list: 1->2->3->4->5

For k = 2, you should return: 2->1->4->3->5

For k = 3, you should return: 3->2->1->4->5

## Solution

Using three pointers to keep remember the head, end and the next head of these blocks. And for each block, using the reverse algorithm of the linked list.

```C++
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(k == 1) return head;
        ListNode* ptr1 = head;
        ListNode* ptr3 = head;
        for(int i=0;i<k-1;i++) {
            head = head->next;
            if (head == NULL) return ptr1;
        }
        ListNode* ptr2 = head;
        while(true){
            ptr3 = ptr2->next;
            if(ptr3 == NULL){
                ptr1 = reverseK(ptr1,k);
                ptr1->next = NULL;
                break;
            }
            for(int i=0;i<k;i++){
                ptr2 = ptr2->next;
                if(ptr2 == NULL) break;
            }
            if(ptr2 != NULL){
                ptr1 = reverseK(ptr1, k);
                ptr1->next = ptr2;
                ptr1 = ptr3;
            }
            else {
                ptr1 = reverseK(ptr1, k);
                ptr1->next = ptr3;
                break;
            };
        }
        return head;
    }
    ListNode* reverseK(ListNode* head, int k){
        ListNode* prev = head;
        ListNode* mid = head->next;
        ListNode* next = head->next->next;
        for(int i=0;i<k-1;i++){
            mid->next = prev;
            prev = mid;
            mid = next;
            if(next == NULL) break;
            else next = next->next;
        }
        return head;
    }
};
```
