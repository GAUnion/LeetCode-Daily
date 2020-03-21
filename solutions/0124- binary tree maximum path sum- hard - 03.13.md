## Problem
Given a non-empty binary tree, find the maximum path sum.

## Solution
For this problem, there are two situations for each node in the maximum path:
 - the maximum path include this node and its left and right children;
 - the maximum path include this node, its parent and its left or right chilren;

For situation one, we set an int ret to record the max value of this situation. For situation two, we use recursion to calculate the result.
```
class Solution {
public:
    int ret;
    int maxPathSum(TreeNode* root) {
        if(!root) return 0;
        ret = root->val;
        ret = max(ret, getMax(root));
        return ret;
    }

    int getMax(TreeNode* root){
        if(root == NULL) return 0;
        int left = max(0,getMax(root->left));
        int right = max(0,getMax(root->right));
        ret = max(ret,left+right+root->val);
        return root->val + max(0,max(left, right));
    }
};
```
