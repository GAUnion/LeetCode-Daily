# 1171. Remove Zero Sum Consecutive Nodes from Linked List

### Problem

Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list. You may return any such answer.

 (Note that in the examples below, all sequences are serializations of `ListNode` objects.)

### Intuition

![image](https://assets.leetcode.com/users/hamlet_fiis/image_1566705933.png)

After delete:

![image](https://assets.leetcode.com/users/hamlet_fiis/image_1566705946.png)



### Solutions

##### Method 1

The idea is, if we calculate sum of values from start and if A and B nodes are having few zero sum nodes between them, they will have the same sum. Eg: In [2,4,2,-6,7], node 2 and node -6 both are having sum as 2, so we can connect node 2 to node 7, essentially removing zero-sum nodes [4,2,-6] from the list.

Have a **HashMap** (**sumNodeMap**) of node and sum till that node (sum --> node).

Need to have a dummy head node, to handle in case the head should be deleted as it is zero sum, eg in [1,-1,4,3]. Traverse the Linked List. In each node, calculate the sum. If the sum exists in **sumNodeMap**, that means the nodes between when we found the same sum and current node are zero sum. So get that old node and point to next node of current node (essentially removing in-between zero-sum nodes). Also, remove those in-between nodes from **sumNodeMap**, to make sure that we don't use the old removed node again in future. Note that this removing process can run for max n (number of nodes) times in total. Thus doesn't affect the O(n) time complexity of the whole program.

```java
    public ListNode removeZeroSumSublists(ListNode head) {
        HashMap<Integer, ListNode> sumNodeMap = new HashMap<>();
        ListNode dummyPreHead = new ListNode(0);
        dummyPreHead.next = head; 
        sumNodeMap.put(0, dummyPreHead);            //Init the stack with prehead.
        ListNode currNode = head;

        int sum = 0;
        
        while(currNode!=null){
            sum += currNode.val;
            if(sumNodeMap.containsKey(sum)){
                ListNode oldNodeWithSameSum = sumNodeMap.get(sum);
                //Old node with same sum
                ListNode toBeRemovedNode = oldNodeWithSameSum.next;         
                //Remove zero-sum in-between nodes from sumNodeMap
                int toBeRemovedSum = sum;
                while(toBeRemovedNode != currNode){
                    toBeRemovedSum = toBeRemovedSum + toBeRemovedNode.val;
                    sumNodeMap.remove(toBeRemovedSum);
                    toBeRemovedNode = toBeRemovedNode.next;
                }
                oldNodeWithSameSum.next = currNode.next;                    
                //Point old node to current next node
            }
            else{
                sumNodeMap.put(sum, currNode);
            }
            
            currNode = currNode.next;
        }
        
        return dummyPreHead.next;
    }
```



##### Method 2

If we don't insist on one pass, we can find the two passes is actually really neat.

Iterate for the first time, calculate the `prefix` sum, and save the it to `seen[prefix]`

Iterate for the second time, calculate the `prefix` sum, and directly skip to last occurrence of this `prefix`

```java
public ListNode removeZeroSumSublists(ListNode head) {
        int prefix = 0;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        Map<Integer, ListNode> seen = new HashMap<>();
        seen.put(0, dummy);
        for (ListNode i = dummy; i != null; i = i.next) {
            prefix += i.val;
            seen.put(prefix, i);
        }
        prefix = 0;
        for (ListNode i = dummy; i != null; i = i.next) {
            prefix += i.val;
            i.next = seen.get(prefix).next;
        }
        return dummy.next;
    }
```

