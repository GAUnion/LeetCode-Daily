### [LeetCode 310 Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)

The actual implementation is similar to the BFS topological sort. In our loop, each time we delete the leaf nodes from our graph, and meanwhile we add the new leaf nodes after deleting their edges to the original leaf nodes. Finally, the nodes would be 1 or 2 left.

Here is a picture expressing the same thing.

![](C:\Users\qiany\Pictures\leetcode310.jpg)

Please note that a tree does not contain any cycle in it. That is why there are at most two nodes can be the roots of Minimum Height Trees. If three nodes are left, then the only possible condition is that there are two leaves.

Java solution：

```java
class Solution {
    public List<Integer> findMinHeightTrees(int n, int[][] edges) {
    	List<Integer> res = new ArrayList<>();
    	if (n <= 0) return res;
        // Corner case
    	if (n == 1) {
            res.add(0);
            return res;
        }
        // Initialize the undirected graph
        List<Set<Integer>> adj = new ArrayList<>(n);
        for (int i = 0; i < n; i++) adj.add(new HashSet<>());
        for (int[] edge : edges) {
            adj.get(edge[0]).add(edge[1]);
            adj.get(edge[1]).add(edge[0]);
        }
		// Create first leaf layer
        List<Integer> leaves = new ArrayList<>();
        for (int i = 0; i < n; i++)
            if (adj.get(i).size() == 1) leaves.add(i);
		// BFS the graph
        while (n > 2) {
            n -= leaves.size();
            List<Integer> newLeaves = new ArrayList<>();
            for (int i : leaves) {
                int j = adj.get(i).iterator().next();
                adj.get(j).remove(i);
                if (adj.get(j).size() == 1) newLeaves.add(j);
            }
            leaves = newLeaves;
        }
        return leaves;  
    }
}
```

Python solution：

```python
class Solution:	
    def findMinHeightTrees(self, n, edges):
        d = collections.defaultdict(set)
        #Initialize the undirected graph
        for u, v in edges:
            d[u].add(v)
            d[v].add(u)
        #BFS the graph
        s = set(range(n))
        while len(s) > 2:
            leaves = set(i for i in s if len(d[i]) == 1)
            s -= leaves
            for i in leaves:
                for j in d[i]:
                    d[j].remove(i)
        return list(s)
```

C++ solution：

```c++
class Solution {
 public:
  vector<int> findMinHeightTrees(int n, vector<pair<int, int>>& edges) {
    // Initialize the undirected graph
    vector<unordered_set<int>> adj(n);
    for (pair<int, int> p : edges) {
      adj[p.first].insert(p.second);
      adj[p.second].insert(p.first);
    }
    // Corner case
    vector<int> current;
    if (n == 1) {
      current.push_back(0);
      return current;
    }
    // Create first leaf layer
    for (int i = 0; i < adj.size(); ++i) {
      if (adj[i].size() == 1) {
        current.push_back(i);
      }
    }
    // BFS the graph
    while (true) {
      vector<int> next;
      for (int node : current) {
        for (int neighbor : adj[node]) {
          adj[neighbor].erase(node);
          if (adj[neighbor].size() == 1) next.push_back(neighbor);
        }
      }
      if (next.empty()) return current;
      current = next;
    }
  }
};
```

