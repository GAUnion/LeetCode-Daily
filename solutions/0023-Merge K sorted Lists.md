# 23 Merge K sorted Lists

## Problem:

![1585879264648](C:\Users\mumu\AppData\Roaming\Typora\typora-user-images\1585879264648.png)

## Solution 1: just compare the value column by column

```java
public ListNode mergeKLists(ListNode[] lists) {
    int min_index = 0;
    ListNode head = new ListNode(0);
    ListNode h = head;
    while (true) {
        boolean isBreak = true;
        int min = Integer.MAX_VALUE;
        for (int i = 0; i < lists.length; i++) {
            if (lists[i] != null) {
                if (lists[i].val < min) {
                    min_index = i;
                    min = lists[i].val;
                }
                isBreak = false;
            }

        }
        if (isBreak) {
            break;
        }
   
        h.next = lists[min_index];
        h = h.next;
        lists[min_index] = lists[min_index].next;
    }
    h.next = null;
    return head.next;
}
```



## Solution 2: use the priority queue:

```java
public ListNode mergeKLists(ListNode[] lists) {
		Comparator<ListNode> cmp;
		cmp = new Comparator<ListNode>() {  
		@Override
		public int compare(ListNode o1, ListNode o2) {
			// TODO Auto-generated method stub
			return o1.val-o2.val;
		}
		};
    	
 
		Queue<ListNode> q = new PriorityQueue<ListNode>(cmp);
		for(ListNode l : lists){
			if(l!=null){
				q.add(l);
			}		
		}
		ListNode head = new ListNode(0);
		ListNode point = head;
		while(!q.isEmpty()){
          
			point.next = q.poll();
			point = point.next;

			ListNode next = point.next;
			if(next!=null){
				q.add(next);
			}
		}
		return head.next;
```

