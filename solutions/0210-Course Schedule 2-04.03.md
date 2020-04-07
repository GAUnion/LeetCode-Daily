# Course Schedule Ⅱ

### Method 1: Topological Sorting

1. Before starting to sort, scan the corresponding storage space (using the adjacency list), and put the node with the degree of 0 into the queue.
2. As long as the queue is not empty, the node with an in-degree of 0 is taken from the head of the team, the node is output to the result set, and the in-degree of all adjacent nodes (the node it points to) of this node is reduced 1. After decrementing by 1, if the degree of the node decremented by 1 is 0, continue to join the team.
3. When the queue is empty, check whether the number of vertices in the result set is equal to the number of courses.

```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        if len(prerequisites) == 0: return [i for i in range(numCourses)]

        in_degrees = [0 for _ in range(numCourses)]
        adj = [set() for _ in range(numCourses)]

        for second, first in prerequisites:
            in_degrees[second] += 1
            adj[first].add(second)

        res = []
        que = []
        for i in range(numCourses):
            if in_degrees[i] == 0:
                que.append(i)

        while que:
            top = que.pop(0)
            res.append(top)

            for successor in adj[top]:
                in_degrees[successor] -= 1
                if in_degrees[successor] == 0:
                    que.append(successor)
                    
        if len(res) != numCourses:
            return []
        return res
```

### Method 2: DFS

We apply DFS to each node and append deepest node to the list. Then we can get the reverse of the course list.

```python
class Solution:
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        from collections import defaultdict

        graph = defaultdict(list)
        for x, y in prerequisites:
            graph[y].append(x)
        res = []
        visited = set()
        
        def dfs(i, being_visited):
            if i in being_visited:
                return False
            if i in visited:
                return True
            visited.add(i)
            being_visited.add(i)
            for j in graph[i]:
                if not dfs(j, being_visited):
                    return False
            being_visited.remove(i)
            res.append(i)
            return True
        
        for i in range(numCourses):
            if not dfs(i, set()): return []
        return res[::-1]
```