# 143. Reorder List

## Problem Description

Given a singly linked list *L*: *L*0→*L*1→…→*L**n*-1→*L*n,
reorder it to: *L*0→*L**n*→*L*1→*L**n*-1→*L*2→*L**n*-2→…

You may **not** modify the values in the list's nodes, only nodes itself may be changed.



## Solutions

### 1. 

Idea:

Suppose we have a list [1,2,3,4,5,6,7]

Steps as follow:

1-Divide the list into front list: [1,2,3,4] and tail list: [5,6,7]
2- Reverse the tail list to [7,6,5]
3- Join front and tail list,
1 ->2 ->3 ->4
->7 ->6 ->5
Result: [1,7,2,6,3,5,4]

```
void reorderList(ListNode* head) {
        if(!head) return;
        ListNode * slowptr,*fastptr;
        slowptr=fastptr=head;
        while(fastptr!=NULL && fastptr->next!=NULL)
        {
            slowptr=slowptr->next;
            fastptr=fastptr->next->next;
        }
        
        ListNode* current = slowptr; 
        ListNode *prev = NULL, *next = NULL; 
  
        while (current != NULL) { 
            next = current->next; 
            current->next = prev; 
            prev = current; 
            current = next; 
        } 
        slowptr = prev; 
        
        ListNode *f1,*f2;
        f1= head->next; f2=slowptr->next;
        while(f2!=NULL)
        {
            head->next= slowptr;
            slowptr->next=f1;
            head=f1;
            slowptr=f2;
            f1=f1->next;
            f2=f2->next;
        } 
    }
```



### 2. Recursive

Go to the last node and recurse back

```
void reorderList(ListNode* head) {
        if(head == NULL) return;
        
        // First find mid point
        ListNode* mid = head, *fast = head;
        while(fast != NULL && fast->next != NULL) {
            mid = mid->next;
            fast = fast->next->next;
        }
        
        reorder(head, mid);
        mid->next = NULL;
    }
    
    ListNode *reorder(ListNode* start, ListNode* end) {
        //Go till the end first and reorder when recursing back
        if(end->next != NULL) start = reorder(start, end->next);
        
        ListNode* t = start->next;
        start->next = end;
        end->next = t;
        return t;
    }
```

