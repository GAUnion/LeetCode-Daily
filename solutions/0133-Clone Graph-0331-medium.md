# Clone Graph

## Problem

Given a reference of a node in a **[connected](https://en.wikipedia.org/wiki/Connectivity_(graph_theory)#Connected_graph)** undirected graph.

Return a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) (clone) of the graph.

Each node in the graph contains a val (`int`) and a list (`List[Node]`) of its neighbors.

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

## Sloutions

### 1.DFS + recursion

Use a HashMap to store all nodes that have been visited and copied to avoid infinite loops. The key of the HashMap is the node in the original graph, and the value is the corresponding node in the clone graph. If the currently visited node is not in the HashMap, a new node is created and stored in HashMap. 
Call the adjacency of each node recursively. The number of recursive calls of each node is equal to the number of adjacencies. Each call returns the clone nodes corresponding to its adjacencies, and finally returns a list of these clone adjacencies and puts them into the adjacency list of the corresponding clone nodes. This way we can clone a given node and its neighbors.

````c++
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return NULL;
        if (map.find(node) == map.end()) {
            map[node] = new Node(node->val, {});
            for (Node* neighbor : node->neighbors) {
                map[node]->neighbors.push_back(cloneGraph(neighbor));
            }
        }
        return map[node];
    }
private:
    unordered_map<Node*, Node*> map;
};
````

### 2.BFS + queue

Take a node from the head of the queue. Iterate through all adjacencies of the node. If an adjacency point has been visited, the adjacency point must be in HashMap. Otherwise, a new node is created and stored in HashMap. Add the cloned adjacencies to the adjacency list of the corresponding nodes in the clone graph.

````c++
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (!node) return NULL;
        queue<Node*> q({node});
        const auto copy = new Node(node->val);
        map[node] = copy;
        while(!q.empty()){
            auto cur = q.front();
            q.pop();
            for (const auto&neighbors : cur->neighbors){
                if (map.find(neighbors) == map.end()){
                    q.push(neighbors);
                    map[neighbors] = new Node(neighbors->val);
                }
                map[cur]->neighbors.push_back(map[neighbors]);
            }
        }
        return map[node];
    }
private:
    unordered_map<Node*, Node*> map;
};
````



**Solutions using Python (BFS, DFS iteratively, DFS recursively)** [on this discussion.](https://leetcode.com/problems/clone-graph/discuss/42314/Python-solutions-(BFS-DFS-iteratively-DFS-recursively).)

