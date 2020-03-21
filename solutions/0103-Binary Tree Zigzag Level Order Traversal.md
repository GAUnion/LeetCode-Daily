# Solution 1: A lazy solution
If you finish Problem 102 `Binary Tree Level Order Traversal' with a BFS-based solution, then congratulation, you need little effort to fit that solution to this problem.

We still add nodes to the result vector level by level. Meanwhile, for those even levels (level 2, level 4, level 6, etc.) we reverse the elements in this level.

Fortunately, `std::reverse` is a STL member function for reversing bidirectional containers. Therefore, only three new lines of code are needed, as follows
```
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if (!root) return ret;
        bool need_flip=false; // New code
        queue<TreeNode*> this_level;
        this_level.push(root);
        while (!this_level.empty())
        {
            vector<int> ret_this_level;
            int temp_size = (int)this_level.size();
            for (int i=0; i<temp_size;i++){
                TreeNode* node = this_level.front();
                this_level.pop();
                ret_this_level.push_back(node->val);
                if (node->left)this_level.push(node->left);
                if (node->right)this_level.push(node->right);
            }
            if (need_flip) reverse(ret_this_level.begin(), ret_this_level.end()); // New code
            need_flip = !need_flip; // New code

            ret.push_back(ret_this_level);
        }
        return ret;
    }
```

# Solution 2: A decent solution without reserve operation
For a more decent solution, we can replace the reverse operation with a random access to the temp vector for each level.
```
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if (!root) return ret;
        bool need_flip=false; // New code
        queue<TreeNode*> this_level;
        this_level.push(root);
        while (!this_level.empty())
        {
            int temp_size = (int)this_level.size();
            vector<int> ret_this_level(temp_size);
            for (int i=0; i<temp_size;i++){
                TreeNode* node = this_level.front();
                this_level.pop();
                if (need_flip) ret_this_level[temp_size-i-1] = node->val; // Change here
                          else ret_this_level[i] = node->val; // Change here
                
                if (node->left)this_level.push(node->left);
                if (node->right)this_level.push(node->right);
            }
            need_flip = !need_flip; // New code

            ret.push_back(ret_this_level);
        }
        return ret;
    }
```