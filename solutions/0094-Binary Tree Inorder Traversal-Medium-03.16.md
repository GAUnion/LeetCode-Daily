# Binary Tree Inorder Traversal

#### Problem

Given a binary tree, return the inorder traversal of its nodes' values.

#### Aim

Get familiar to two different traversal methods: recursion and iteration.

**Plus**: same problem could be on preorder(LC144-medium) and postorder(LC145-hard) traversal.

------

#### Solution 1 : Recursion

As we know, the order of "inorder traversal" is :```left node->self->right node```. So the intuitive method is recursively call the left tree, itself, and its right tree (see below). Very Simple, very neat.

However, at most times that's not what interviewee wants to see, s/he want something harder, thus we also need to implement it in non-recursion way.

------

#### Solution 2: Iteration

Just like the hided stack of recursion, in iteration, we need to build a stack ourselves, to save the node which we visited but not print.

The order is the same, while there is still node unvisited(node != NULL) or unprinted(stack != empty):

1. LEFT: while the node is not NULL, push it to stack and go to it's left node.
2. SELF: get the pop of stack, print it and 
3. RIGHT: visit its right node

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
private:
    vector<int> res;
public:
    void inorderTraversalRec(TreeNode* root) {
        if(root->left != NULL)
            inorderTraversalRec(root->left);
        res.push_back(root->val);
        if(root->right != NULL)
            inorderTraversalRec(root->right);
    }
    
    void inorderTraversalIte(TreeNode* root) {
        stack<TreeNode*> s;
        while(root != NULL || !s.empty()) {
            while(root != NULL) {
                s.push(root);
                root = root->left;
            }
            root = s.top();
            s.pop();
            res.push_back(root->val);
            root = root->right;
        }
    }
    
    vector<int> inorderTraversal(TreeNode* root) {
        if(root == NULL) return res;
        inorderTraversalRec(root);
        //inorderTraversalIte(root);
        return res;
    }
};
```

