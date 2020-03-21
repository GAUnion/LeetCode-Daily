# Construct Binary Tree from Inorder and Postorder Traversal

### Problem

Given inorder and postorder traversal of a tree, construct the binary tree.

### Solution

We use recursion to solve this problem. 

First, the root node value is the last element in the postorder traversal.

For left child tree and right child tree, we use the same approach to get the node value.

Be careful about the different positions of the root node in inorder and postorder traversal.



```
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        if not postorder:
            return None
        root = TreeNode(postorder[-1])
        n = inorder.index(root.val)
        root.left = self.buildTree(inorder[:n],postorder[:n])
        root.right = self.buildTree(inorder[n+1:],postorder[n:-1])
        return root
```

