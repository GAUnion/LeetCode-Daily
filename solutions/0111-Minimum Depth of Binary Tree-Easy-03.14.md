###  LeetCode 111 Minimum Depth of Binary Tree
**Problem:** Given a binary tree, find its *minimum depth*.

*The minimum depth* is the number of nodes along the shortest path from the root node down to the nearest leaf node.
Note: A leaf is a node with no children.

In order to understand the concept of minimum depth better, we give some test cases.

Input:

        [3,9,20,null,null,15,7]
        [1,2]
        [2]
        []
Output:

        2
        2
        1
        0

**Summary**
If the tree is null, the minimum depth is 0.
If the tree only contain one node, the minimum depth is 1.
If the tree contains two nodes, it is impossible that they are both leaf nodes. The the minimum depth is 2.

**Solution 1**  recursion

Using recursion to solve this problem. Find the minimum depth of the left subtree and the right subtree. The result is the minimum of them plus one. The special case should be noted that if either left or right subtree is null, the general solution is invalid.

```java
public int minDepth(TreeNode root) {
    if (root == null) return 0;
    int left = minDepth(root.left);
    int right = minDepth(root.right);
    if (left == 0 || right == 0)  return left + right + 1;
    return Math.min(left, right) + 1;
}
```

```java
public int minDepth(TreeNode root) {
    if(root == null) return 0;
    int left = minDepth(root.left);
    int right = minDepth(root.right);
    return 1 + ((Math.min(left, right) > 0) ? Math.min(left, right) : Math.max(left, right));
}
```

**Solution 2**  BFS

```java
public int MinDepth(TreeNode root) {
        if(root == null) return 0;
        int depth = 1;
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        queue.offer(root);
        while(!queue.isEmpty())
        {
            int size = queue.size();
            while(size-- > 0)
            {
                TreeNode temp = queue.poll();
                if(temp.left == null && temp.right == null)  return depth;
                if(temp.left != null) queue.offer(temp.left);
                if(temp.right != null) queue.offer(temp.right);
            }
            depth++;
        }
        return depth;
    }
```

