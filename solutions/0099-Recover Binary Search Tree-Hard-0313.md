# 99. Recover Binary Search Tree

## Problem: 

Two elements of a binary search tree (BST) are swapped by mistake. Recover the tree without changing its structure.

**Follow up:** A solution using O(*n*) space is pretty straight forward. Could you devise a constant space solution?

## Solutions

### **1. Inorder Traversal**

The inorder traversal of a binary search tree is an ascending sequence. In this problem, the normal ascending sequence will be partially descended. As long as we find the descending point and exchange the corresponding value, we can recover the binary search tree.

If there is a descending pair in the inorder traversal, the two nodes need to be swapped; if there are two descending pairs, the previous node of the first pair and the latter node of the second pair need to be swapped.

`````python
class Solution:
    def __init__(self):
        self.res = []
        
    def recoverTree(self, root: TreeNode) -> None:
        self.mid(root)
        node1 = None
        node2 = None
        for i in range(len(self.res)-1):
            if self.res[i].val > self.res[i+1].val and node1 == None:
                node1 = self.res[i]
                node2 = self.res[i+1]
            elif self.res[i].val > self.res[i+1].val and node1 != None:
                node2 = self.res[i+1]                
        node1.val, node2.val = node2.val, node1.val
        
    def mid(self,root):
        if root is not None:
            self.mid(root.left)
            self.res.append(root)
            self.mid(root.right)
`````

### 2. Morries Traversal

Using Morris Traversal, we can traverse the tree using O (1) space and without using stack and recursion. The idea of Morris Traversal is based on [Threaded Binary Tree](http://en.wikipedia.org/wiki/Threaded_binary_tree). In this traversal, there is no need to assign pointers for each node to its predecessor and successor. We only need to use the left and right null pointers in the leaf nodes to point to the predecessor or successor in a certain order. 

1. Initialize current as root                                                                                                                             
2. While current is not NULL
   If the current does not have left child
      a) Print current’s data
      b) Go to the right, i.e., current = current->right
   Else
      a) Make current the right child of the rightmost node in current's left subtree
      b) Go to this left child, i.e., current = current->left`

![img](https://images0.cnblogs.com/blog/300640/201306/14214057-7cc645706e7741e3b5ed62b320000354.jpg)

The illustration is based [on this blog](https://www.cnblogs.com/AnnieKim/archive/2013/06/15/morristraversal.html).

More information about Morris inorder traversal [on this video](https://www.youtube.com/watch?v=wGXB9OWhPTg).

`````python
class Solution:
    def recoverTree(self, root):
        node1 = node2 = predecessor = lastnode = None
        
        while root:
            if root.left:       
                predecessor = root.left
                while predecessor.right and predecessor.right != root:
                    predecessor = predecessor.right
 
                if predecessor.right is None:
                    predecessor.right = root
                    root = root.left
                else:
                    if lastnode and root.val < lastnode.val:
                        node2 = root
                        if node1 is None:
                            node1 = lastnode 
                    lastnode = root
                    
                    predecessor.right = None
                    root = root.right

            else:
                if lastnode and root.val < lastnode.val:
                    node2 = root
                    if node1 is None:
                        node1 = lastnode 
                lastnode = root
                
                root = root.right
        
        node1.val, node2.val = node2.val, node1.val  
`````

Time complexity: O(N)
Space complexity: O(1)