# 02. Add Two Numbers

## Problem Description

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

## Solution

#### Elementary Math

Very classic problem which is frequent in interview.

Just like how you would sum two numbers on a piece of paper, we begin by summing the least-significant digits. However, sometimes summing two digits may "overflow". In this case, we set the current digit to its unit and bring over the ***carry*** = tens to the next iteration. ***carry*** must be either 0 or 1 because the largest possible sum of two digits (including the carry) is 9 + 9 + 1 = 19.

```java
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        int plus = 0;
        ListNode head = new ListNode(0);
        ListNode headDummy = head;
        while (l1 != null || l2 != null || plus != 0) {
            int num1 = 0, num2 = 0;
            if (l1 != null) {
                num1 = l1.val;
                l1 = l1.next;
            }
            if (l2 != null) {
                num2 = l2.val;
                l2 = l2.next;
            }
            int sum = num1 + num2 + plus;
            head.next = new ListNode(sum % 10);
            plus = sum / 10;
            head = head.next;
        }
        return headDummy.next;
    }
```

##### Time Complexity:

O(N)