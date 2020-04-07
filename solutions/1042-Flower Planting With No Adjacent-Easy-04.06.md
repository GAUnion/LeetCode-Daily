# Flower Planting With No Adjacent
You have `N` gardens, labelled `1` to `N`.  In each garden, you want to plant one of 4 types of flowers.

`paths[i] = [x, y]` describes the existence of a bidirectional path from garden `x` to garden `y`.

Also, there is no garden that has more than 3 paths coming into or leaving it.

Your task is to choose a flower type for each garden such that, for any two gardens connected by a path, they have different types of flowers.

Return **any** such a choice as an array `answer`, where `answer[i]` is the type of flower planted in the `(i+1)`-th garden.  The flower types are denoted 1, 2, 3, or 4.  It is guaranteed an answer exists.

### Mehod 1: Brute-Force
Basic idea: adjust the color of nodes whenever two nodes have the same color. Initialy, every node has color 1.

```java
public int[] gardenNoAdj(int n, int[][] paths) {
        int[] result = new int[n];
        Arrays.fill(result, 1);
        boolean stop = false;
        do {
            stop = true;
            for(int[] edge: paths) {
                int u = Math.min(edge[0], edge[1]);
                int v = Math.max(edge[0], edge[1]);
                if (result[u - 1] == result[v - 1]) {
                    stop = false;
                    result[v - 1] = result[v - 1] == 4 ? 1 : result[v - 1] + 1;
                }
            }
        }
        while(!stop);
        return result;
    }	
```
***
### Method 2: Greedy
**Intuition**
Because there is no node that has more than 3 neighbors, so painter can greedily paint nodes one by one. And there is always one possible color to choose.

**Complexity**

Time O(N)  
Space O(N)

```java
public int[] gardenNoAdj(int N, int[][] paths) {
        Map<Integer, Set<Integer>> G = new HashMap<>();
        for (int i = 0; i < N; i++) G.put(i, new HashSet<>());
        for (int[] p : paths) {
            G.get(p[0] - 1).add(p[1] - 1);
            G.get(p[1] - 1).add(p[0] - 1);
        }
        int[] res = new int[N];
        for (int i = 0; i < N; i++) {
            int[] colors = new int[5];
            for (int j : G.get(i))
                colors[res[j]] = 1;
            for (int c = 4; c > 0; --c)
                if (colors[c] == 0)
                    res[i] = c;
        }
        return res;
    }
```