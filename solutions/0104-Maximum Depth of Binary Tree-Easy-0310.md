# 104 Maximum Depth of Binary Tree

## Problem Description

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Note: A leaf is a node with no children.

## Solutions

***
### 1. Recursion

The maximum depth of each node is equal to the greater of the maximum depth of the left child node and the maximum depth of the right child node, plus one.<br />
Based on this, it can be solved recursively.

```C++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==NULL) 
            return 0;
        int left_depth = 0, right_depth = 0;
        if (root->left!=NULL) 
            left_depth = maxDepth(root->left);
        if (root->right!=NULL) 
            right_depth = maxDepth(root->right);
        return max(left_depth, right_depth) + 1;
    }
};
```

***
### 2. Iteration （Hierarchical traversal）

a. Build a queue and put the tree in <br />
b. Using the characteristics of the queue, the elements of the tree are read layer by layer, and each layer is read, the number of layers is increased by 1.<br />
c. Until all layers are traversed, the number of output layers<br />

```Python
class Solution:
    def maxDepth(self, root: TreeNode) -> int:
        if root == None:
            return 0
        stack = [root]
        i = 0
        while len(stack) != 0:
            n = len(stack)
            i += 1
            for k in range(n):
                temp = stack.pop(0)
                if temp.left:
                    stack.append(temp.left)
                if temp.right:
                    stack.append(temp.right)
        return i 
```

