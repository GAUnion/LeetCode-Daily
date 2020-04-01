# Crouse Schedule

### Method: Indegree Table(BFS)

Key: The course schedule is whether a Directed Acyclic Graph(DAG) or not.

1. Calculate the indegree of every node in the course schedule and build a indegree table.
2. Put all the nodes with 0 indegree into a queue
3. When the queue is not empty, dequeue the first node and subtract all the adjacent nodes of it by 1; when the indegree of the adjacent node is 0 after subtraction, put it into the queue
4. When a node dequeues, numCourses --
5. If the course schedule is not a DAG, there must be a node whose indegree can never be subtracted to 0. So, if numCourses equals 0, the course can be arranged. Otherwise, it can't be arranged.

```python
from collections import deque

class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        indegrees = [0 for _ in range(numCourses)]
        adjacency = [[] for _ in range(numCourses)]
        queue = deque()
        # Get the indegree and adjacency of every course.
        for cur, pre in prerequisites:
            indegrees[cur] += 1
            adjacency[pre].append(cur)
        # Get all the courses with the indegree of 0.
        for i in range(len(indegrees)):
            if not indegrees[i]: queue.append(i)
        # BFS TopSort.
        while queue:
            pre = queue.popleft()
            numCourses -= 1
            for cur in adjacency[pre]:
                indegrees[cur] -= 1
                if not indegrees[cur]: queue.append(cur)
        return not numCourses
```

### Method: DFS

1. Use flag to judge the state of every node: if the flag == 0, the node hasn't been visited. if the flag == -1, it has been visited by other nodes. if the flag == 1, it is visited by current node.
2. Execute DFS to every node and judge whether there is a ring.  
    When flag[i] == -1, the node has been visited by the DFS of other nodes, which don't need DFS again. Return True.  
    When flag[i] == 1, the node has been visited twice in this DFS, which means there is a ring. Return False  
    Assign the flag of current visited node i: flag[i] as 1, which means it has been visited in this DFS  
    Recursively visit all the adjacent nodes of node i, return False when ring is detected.  
    When the adjacent nodes of the node i have all been visited and no rings has been detected. Then assign the flag[i] as -1 and return True.  
3. If there is no ring in the graph, return True.

```python
class Solution:
    def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
        def dfs(i, adjacency, flags):
            if flags[i] == -1: return True
            if flags[i] == 1: return False
            flags[i] = 1
            for j in adjacency[i]:
                if not dfs(j, adjacency, flags): return False
            flags[i] = -1
            return True

        adjacency = [[] for _ in range(numCourses)]
        flags = [0 for _ in range(numCourses)]
        for cur, pre in prerequisites:
            adjacency[pre].append(cur)
        for i in range(numCourses):
            if not dfs(i, adjacency, flags): return False
        return True
```




