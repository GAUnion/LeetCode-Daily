# Unique Binary Search Trees II

## Recursive method

Once the tree root is designated, all the nodes with values smaller than the root will be in the left sub-tree, and all the nodes with larger values will be in the right sub-tree. So a recursive function can be designed to solve this problem. If you want to solve the problem f(begin,end), you can assign i as the root, then solve f(begin,i-1) and f(i+1,end), and adopt the returned trees as the left subtree and right subtree respectively.

```python
class Solution:
  def generateTrees(self, n):
    def node(x, l, r): 
      t = TreeNode(x)
      t.left = l
      t.right = r
      return t
    
    def gen(begin, end):
      if begin>end:
      		return [None]
      res=[]
      for i in range(begin, end+1):
            for l in gen(begin, i - 1):
                for r in gen(i + 1, end):
		            res.append(node(i, l, r))
      return res
        
    return gen(1, n) if n > 0 else []
```



## Dynamic Programming

In the recursive method, some f(a,b) will be calculated more than once, causing the method to be very time consuming. With dynamic programming, you can first calculate the trees with less nodes (with small a-b), store the value, and use them when calculate the trees with more nodes.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
def node(x,l=None,r=None):
    t=TreeNode(x)
    t.left=(l)
    t.right=(r)
    return t

class Solution:
    def generateTrees(self, n: int) -> List[TreeNode]:
        array=[[[] for j in range(n+2)] for i in range(n+1)]
        for i in range(n+2):
            array[0][i].append(None)
        for i in range(1,n+1):
            array[1][i].append(node(i))
        for i in range(2,n+1):
            for j in range(1,n+2-i):
                for k in range(i):
                    for l in array[k][j]:
                        for r in array[i-1-k][j+k+1]:
                            array[i][j].append(node(j+k,l,r))
        return array[n][1]
```

