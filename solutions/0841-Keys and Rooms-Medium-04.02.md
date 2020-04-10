# Keys and Rooms
## Problem Description

There are `N` rooms and you start in room `0`. Each room has a distinct number in `0, 1, 2, ..., N-1`, and each room may have some keys to access the next room. 

Formally, each room `i` has a list of keys `rooms[i]`, and each key `rooms[i][j]` is an integer in `[0, 1, ..., N-1]` where `N = rooms.length`. A key `rooms[i][j] = v` opens the room with number `v`.

Initially, all the rooms start locked (except for room `0`). 

You can walk back and forth between rooms freely.

Return `true` if and only if you can enter every room.

**Example 1:**

```
Input: [[1],[2],[3],[]]
Output: true
Explanation:  
We start in room 0, and pick up key 1.
We then go to room 1, and pick up key 2.
We then go to room 2, and pick up key 3.
We then go to room 3.  Since we were able to go to every room, we return true.
```

**Example 2:**

```
Input: [[1,3],[3,0,1],[2],[0]]
Output: false
Explanation: We can't enter the room with number 2.
```

**Note:**

1. `1 <= rooms.length <= 1000`
2. `0 <= rooms[i].length <= 1000`
3. The number of keys in all rooms combined is at most `3000`.

## Solution

Since we need to traverse the room, we could regard it as a graph. We could assume rooms as vertices and keys as edges.  Thus we could traverse as much rooms as we could, mark if we visit one room and never come back, then determine if we traverse each room.

### BFS

So the intuitive solution for me is Breath-First-Search BFS. We use a priority queue `todo` to restore the rooms which we got the key but haven't visited yet. To prevent visit to room we've already visited, I use a list`visited` to restore information and `numvisited` to count the numbers of visited rooms. Since when we finished visiting all possible rooms, we could compare `numvisited` and `rooms.size` to decide if we traverse all rooms.

```c++
class Solution {
public:
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int len = rooms.size(), numvisited = 0;
        vector<int> visited(len);
        queue<int> todo;
        todo.push(0);
        visited[0]++;
        numvisited++;
        while(!todo.empty()) {
            int cur = todo.front();
            todo.pop();
            for(auto k: rooms[cur]) {
                if(visited[k] == 0) {
                    todo.push(k);
                    numvisited++;
                    visited[k]++;
                }
            }
        }
        return numvisited == len;
    }
};
```



### DFS

Actually, we could simply change priority queue to stack, to achieve DFS traverse on the graph, since there is no order we should obey when traversing. But to make it more 'DFS', I still implement it with recursion, although it's the same idea of irritation.

```c++
class Solution {
public:
    void dfs(vector<vector<int>>& rooms, vector<int> &visited, int cur, int &numvisited) {
        visited[cur]++;
        numvisited++;
        for(auto k: rooms[cur])
            if(visited[k] == 0)
                dfs(rooms, visited, k, numvisited);
    }
    bool canVisitAllRooms(vector<vector<int>>& rooms) {
        int len = rooms.size(), numvisited = 0;
        vector<int> visited(len);
        dfs(rooms, visited, 0, numvisited);
        return numvisited == len;
    }
};
```