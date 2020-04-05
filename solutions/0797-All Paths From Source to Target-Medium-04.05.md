# 797. All Paths From Source to Target

## Problem Description

Given a directed, acyclic graph of N nodes.  Find all possible paths from node 0 to node N-1, and return them in any order.

The graph is given as follows:  the nodes are 0, 1, ..., graph.length - 1.  graph[i] is a list of all nodes j for which the edge (i, j) exists.

## Solutions

***
### 1. DFS

Suppose there are N nodes in the graph. During the search, if we reach node N-1, then the path at this time is {N-1}; otherwise, if we reach other nodes, the path is {node } Plus {all paths from nei to N-1}, where nei is the node directly adjacent to node.

```C++
class Solution {
    public List<List<Integer>> allPathsSourceTarget(int[][] graph) {
        return solve(graph, 0);
    }

    public List<List<Integer>> solve(int[][] graph, int node) {
        int N = graph.length;
        List<List<Integer>> ans = new ArrayList();
        if (node == N - 1) {
            List<Integer> path = new ArrayList();
            path.add(N-1);
            ans.add(path);
            return ans;
        }

        for (int nei: graph[node]) {
            for (List<Integer> path: solve(graph, nei)) {
                path.add(0, node);
                ans.add(path);
            }
        }
        return ans;
    }
}
```

***
### 2. BFS

Simulate a queue, do not exit and continue the loop after finding the end point

```Python
class Solution(object):
    def allPathsSourceTarget(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: List[List[int]]
        """
        end = len(graph) - 1
        queue = list()
        res = list()
        for x in graph[0]:
            queue.append([[0], x])
        while len(queue) > 0:
            cur_node = queue[0]
            prev_path, node_num = cur_node
            if node_num == end:
                res.append(prev_path + [node_num])
            else:
                cur_path = prev_path + [node_num]
                for x in graph[node_num]:
                    queue.append([cur_path, x])
            queue.pop(0)
        return res
```

