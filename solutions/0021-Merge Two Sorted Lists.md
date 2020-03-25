# Merge Two Sorted Lists
Just nothing to describe. Like a one-round merge sort.

Here I use a trick to prevent from judging edge case of the first node. I set up a `head node' (-10007 in code) which is meanless in element value. Then each pCur will not be null pointer. When returning the result, just return ret->next.

``` 
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode* ret;
        ListNode* pCur;
        // Notice: Don't write as: 
        // ListNode* ret, pCur; 
        // which is wrong.
        ret = new ListNode(-10007);
        pCur = ret;
        while (l1 && l2){
            ListNode* newNode;
            if (l1->val > l2->val) {newNode = new ListNode(l2->val); l2=l2->next;}
                             else  {newNode = new ListNode(l1->val); l1=l1->next;}
                pCur->next = newNode;
                pCur = pCur->next;                
        }
        if (l1) pCur->next = l1;
        if (l2) pCur->next = l2;
        // while (l1) {pCur->next = new ListNode(l1->val); pCur = pCur->next; l1=l1->next;}
        // while (l2) {pCur->next = new ListNode(l2->val); pCur = pCur->next; l2=l2->next;}
        return ret->next;
    }
```