# Binary Tree Right Side View

**Problem**: Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

## Depth-First-Search 

It's a recursion method. Start with the root node. Every recursion step start with check whether it is None and whether it is the first node of current depth. If it is none, return nothing. If it is the first node of current depth, append a random number,say 0, to the result list. Assign the value of current node to the res\[depth\]. Then, executed dfs(root.left, depth + 1) and dfs(root.right, depth + 1). Such order can make sure that the latest assigned value of the result\[depth\] is the right most node's value of this depth.

Python code

```python
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        res = []

        def dfs(root, depth):
            if not root:
                return 
            if len(res) <= depth:
                res.append(0)
            res[depth] = root.val
            dfs(root.left, depth + 1)
            dfs(root.right, depth + 1)
        
        dfs(root, 0)
        return res
```

Reached double 100% in python

## Breath-First-Search

The queue stores all the node of the current depth. nxt stores all the node of the next depth. Start with the root node. With every node in queue, append its left child node and right child node to the nxt. It promises that the order of the nodes in the nxt is as same as it is on the tree graph. Assign the value of the right most node in the queue to res. 

Python Code

 ````python
class Solution:
    def rightSideView(self, root: TreeNode) -> List[int]:
        if not root: return []
        res = []

        def bfs(root):
            queue = [root]
            while queue:
                nxt = []
                for node in queue:
                    if node.left:
                        nxt.append(node.left)
                    if node.right:
                        nxt.append(node.right)
                res.append(queue[-1].val)
                queue = nxt
        bfs(root)
        return res
 ````

