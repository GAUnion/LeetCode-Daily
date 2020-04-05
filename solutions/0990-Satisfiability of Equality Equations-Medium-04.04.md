# 990 - Satisfiability of Equality Equations

### Description 

Given an array equations of strings that represent relationships between variables, each string `equations[i]` has length `4` and takes one of two different forms: `"a==b"` or `"a!=b"`. Here, `a` and `b` are lowercase letters (not necessarily different) that represent one-letter variable names.

Return `true` if and only if it is possible to assign integers to variable names so as to satisfy all the given equations.

### Solution 1: DFS

The basic idea is to check whether there are any inequations conflicting with other equations. 

1. We use a map `visited` to store all the visited letters, they are all initiated as false.
2. The `dfs` function checks whether two letters are equal, when checking the equation between `from` and `to`, it will first set the visited bit to true. Then, it will recursively check all the letters equals to `from`, and it will check the visited bit to avoid any rings. If there is any path to the equation, it will return true, otherwise, it will return false.
3. We loop through all the inequations in the vector, and search for conflicting equations.

**C++ Implementation**

```C++
class Solution {
public:
    bool equationsPossible(vector<string>& equations) {
        unordered_map<char, vector<char>> map;
        vector<vector<char>> unequals;
        unordered_map<char, bool>   visited;
        for(auto e : equations){
            visited[e[0]] = false;
            visited[e[3]] = false;
            if(e[1]=='!'){
                if(e[0]!=e[3])
                    unequals.push_back({e[0], e[3]});
                else
                    return false;
            }
            else if(e[0] != e[3]){
                map[e[0]].push_back(e[3]);
               	map[e[3]].push_back(e[0]);
            }
        }

        for(auto u : unequals){
            if(dfs(u[0], u[1], visited, map)) 
                return false;
        }
        return true;
    }
    
    bool dfs(char from, char to, unordered_map<char, bool>& visited, unordered_map<char, vector<char>>& map){
        if(from==to)    return true;
        visited[from] = true;
        for(char c : map[from]){
            if(!visited[c] && dfs(c, to, visited, map))
                return true;
        }
        visited[from] = false;
        return false;
    }
};
```

### Solution 2: Union find

We can see each letter as a node, then there will be 26 nodes in the graph. And each equation represents a connection between a pair of nodes. If two nodes are connected, we put them in a union. After we have grouped every nodes, we check all the inequations, and two unequal nodes should be in two different union.

```c++
class Solution {
public:
    bool equationsPossible(vector<string>& equations) {
        vector<int> uf(26,0);
        for(int i=0; i<26; i++){
            uf[i] = i;
        }
        for(int i=0; i<equations.size(); i++){
            if(equations[i][1]=='='){
                uf[find(uf,equations[i][0]-'a')] = find(uf, equations[i][3]-'a');
            }
        }
        for(int i=0; i<equations.size(); i++){
            if(equations[i][1]=='!'){
                if(find(uf,equations[i][0]-'a')==find(uf,equations[i][3]-'a'))
                    return false;
            }
        }
        return true;
    }
    
    int find(vector<int>& uf, int i){
        while(uf[i]!=i)
            i = uf[i];
        return i;
    }
};
```
