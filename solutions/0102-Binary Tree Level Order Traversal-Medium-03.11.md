# Binary Tree Level Order Traversal

------

#### Problem

Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).

------

#### Solution 1 : Depth-First-Search + Recursive

First of all, we can consider the problem that how to print the node value of one binary tree at certain level (from left to right). Normally we would use recursive algorithm: if we want the node at depth = k, we can transfer this problem to "find the node at depth = k -1 from the left and right subtree of root in the binary tree".

In this way, we can write the levelHelper() function. Its parameter is **node** and **depth**. Everytime we add one node's val at depth = k to its corresponding list whose order is k. Then we can call the levelHelper() to process its left child and right child, and pass the parameter that depth= k + 1.  

Besides, we use the two-dimension list to store the ans. And if the depth is larger than the list, it is because we detect the new level then we need to add a new list in the two-dimension list.

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        levelHelper(res, root, 0);
        return res;
    }
    
    public void levelHelper(List<List<Integer>> res, TreeNode root, int height) {
        if (root == null) return;
        if (height == res.size()) {
            res.add(new LinkedList<Integer>());
        }
        res.get(height).add(root.val);
        levelHelper(res, root.left, height+1);
        levelHelper(res, root.right, height+1);
    }
}
```

The recursive method is clean and neat. Howerver it usually take more time and space than the iterative method. (In this problem, two methods' efficiency are very close)

#### Solution 2: Breadth-First-Search + Iterative:

We could start from the root node, than push the node at every level to the ***array.*** Besides, we need one pointer ***curr*** to record the current index in the array and another pointer ***end*** to point next one of current level's last node's postion. In this way, if ***curr == end***, we can know that the function already visit the all the node in one level. Then if ***array*** still have node which is not visited, function continues visitting the node of  next level (until all the node is visited).

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        List<TreeNode> nodeList = new ArrayList<>(); // record all node in tree
        nodeList.add(root);
        int curr = 0;
        int end = 1; //point to the next pos of current level's last node
        while (curr < nodeList.size()) {
            List<Integer> lineList = new ArrayList<>(); //record certain level nodes' value
            end = nodeList.size();
            while (curr < end) {
                TreeNode node = nodeList.get(curr);
                if (node.left != null) {
                    nodeList.add(node.left);
                }
                if (node.right != null) {
                    nodeList.add(node.right);
                }
                lineList.add(node.val);
                curr++;
            }
            res.add(lineList);
        }
        return ansList;
    }
}
```

