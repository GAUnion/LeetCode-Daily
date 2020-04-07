# Description

In this problem, a tree is an undirected graph that is connected and has no cycles.

The given input is a graph that started as a tree with N nodes (with distinct values 1, 2, ..., N), with one additional edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] with u < v, that represents an undirected edge connecting nodes u and v.

Return an edge that can be removed so that the resulting graph is a tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array. The answer edge [u, v] should be in the same format, with u < v.

# Solution

The first edge that constructs a cycle should be the redundant edge. We can use a disjoint find-union structure to detect the redundant edge.

```
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> ret;
        int x[1005];
        initUnion(x);
        for (auto edge:edges){
            if (!findUnion(edge[0], edge[1], x)){
                addUnion(edge[0], edge[1], x);
            }
            else ret = edge;
        }
        return ret;
```