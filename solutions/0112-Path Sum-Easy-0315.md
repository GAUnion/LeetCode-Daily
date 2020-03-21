### Leetcode 0112 [Path Sum](https://leetcode-cn.com/problems/path-sum/)

#### Problem

Given a binary tree and a sum, determine if the tree has a **root-to-leaf** path such that adding up all the values along the path equals the given sum.

**Struct**

```c++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```

#### Example

Given the below binary tree and sum = 22

```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```

#### Solution 1: Recursion & Substraction

------

Search the whole tree from the root node using recursion and substract the value of each "root" node of the left tree. 

If the current node is leaf, 

​	① sum==root->val, then the sum is val, return true; 

​	② sum != root->val, then the sum is not val, return false;

If the current node is not leaf, use recursion on both its left and right child node to decide whether either of them leads to a path, that is, node.val + hasPathSum(childnode)==sum. If one of the equation is correct, return true.

```c++
bool hasPathSum(TreeNode* root, int sum) {
    if (root == NULL) return false;
    if (root->left == NULL & root->right == NULL) return sum - root->val == 0;
    return hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val);
```

#### Solution 2: Recursion & Addition(DFS)

------

```c++
public:
    int flag=0;
    void dfs(TreeNode* root,int cnt,int sum){
        if (root == NULL) return false;
        if (root->left == NULL & root->right == NULL){
            if (sum == cnt+root->val) flag = 1;
            return false;
        }
        dfs(root->left,cnt+root->val,sum);
        dfs(root->right,cnt+root->val,sum);
    }
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL) return false;
        dfs(root,0,sum);
        if (flag) return true;
        return false;
    }
```

Also search the whole tree from the root node using recursion, only use a count(cnt) as the addition of all the past nodes from the root. Use a flag to compare, and the flag turns to 1 only when cnt + root->val == sum, which means that there is a path.