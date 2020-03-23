<<<<<<< Updated upstream
# 100. Same Tree
**Problem**: Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

##Solutions
To find whether two binary trees are the same, we can traverse each node of these two trees, if any node's value is different from its corresponding node on another tree, function ends and returns false, until every node is judged, the function returns true. There are commonly four ways to traverse a binary tree: preorder traversal, midorder traversal, postorder traversal, and hierarchical traversal. Each method has recursive and non-recursive forms when programming. Here we take preorder traversal (recursion form) and hierarchical traversal (non-recursive form) for example.

### **1.Recursion**

The function determines whether the current node of both trees are the same, and do the same to their left and right child. Notice that only when all the nodes are traversed that the function returns true, so the condition would be the current node has no element.

```python
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q: 
            return True
        if type(p) != type(q):
            return False
        if p.val != q.val: 
            return False
        return self.isSameTree(p.left,q.left) and self.isSameTree(p.right, q.right)
```

Time complexity: O(N)

Space complexity: O(log(N))(fully balanced binary tree)~O(N)(completely unbalanced binary tree)

### 2. Iteration--Queue

Also, we can use queue to traverse the trees.

```python
class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        def check(p, q):
            if not p and not q:
                return True
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        queue = [(p, q)]
        while len(queue):
            p, q = queue.pop(0)
            if not check(p, q):
                return False
            if p:
                queue.append((p.left, q.left))
                queue.append((p.right,q.right))
        return True  
```

Time complexity: O(N)

=======
# 100. Same Tree
**Problem**: Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.

##Solutions
To find whether two binary trees are the same, we can traverse each node of these two trees, if any node's value is different from its corresponding node on another tree, function ends and returns false, until every node is judged, the function returns true. There are commonly four ways to traverse a binary tree: preorder traversal, midorder traversal, postorder traversal, and hierarchical traversal. Each method has recursive and non-recursive forms when programming. Here we take preorder traversal (recursion form) and hierarchical traversal (non-recursive form) for example.

### **1.Recursion**

The function determines whether the current node of both trees are the same, and do the same to their left and right child. Notice that only when all the nodes are traversed that the function returns true, so the condition would be the current node has no element.

```python
class Solution:
    def isSameTree(self, p: TreeNode, q: TreeNode) -> bool:
        if not p and not q: 
            return True
        if type(p) != type(q):
            return False
        if p.val != q.val: 
            return False
        return self.isSameTree(p.left,q.left) and self.isSameTree(p.right, q.right)
```

Time complexity: O(N)

Space complexity: O(log(N))(fully balanced binary tree)~O(N)(completely unbalanced binary tree)

### 2. Iteration--Queue

Also, we can use queue to traverse the trees.

```python
class Solution(object):
    def isSameTree(self, p, q):
        """
        :type p: TreeNode
        :type q: TreeNode
        :rtype: bool
        """
        def check(p, q):
            if not p and not q:
                return True
            if not q or not p:
                return False
            if p.val != q.val:
                return False
            return True
        
        queue = [(p, q)]
        while len(queue):
            p, q = queue.pop(0)
            if not check(p, q):
                return False
            if p:
                queue.append((p.left, q.left))
                queue.append((p.right,q.right))
        return True  
```

Time complexity: O(N)

>>>>>>> Stashed changes
Space complexity: O(log(N))(fully balanced binary tree)~O(N)(completely unbalanced binary tree)