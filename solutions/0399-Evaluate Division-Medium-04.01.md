### 0339 Evaluate Division

#### Problem

Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

**Example:**

```
Given a / b = 2.0, b / c = 3.0.
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .
return [6.0, 0.5, -1.0, 1.0, -1.0 ].

The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:

equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```

PS: about 带权并查集

#### Solution

#### 	——Graph + DFS

```c++
class Solution {
public:
    // try to find a route from start to end
    // temp is the current node
    // ans records the total weight of the route up till now
    bool found = false;
    // dfs search path
    void dfs(const string& start, const string& end, string& temp,  unordered_map<string, vector<pair<string, double>>>& g, double& ans, unordered_set<string>& visited){
        if (found) return;
        if (temp == end){
            found = true;
            return;
        }
        string back = temp;
        for(auto it = g[temp].begin(); it != g[temp].end(); it++){
            if(visited.found(it->first) == visited.end()){
                visited.insert(it->first);
                ans *= it->second;
                temp = it->first;
                dfs(start, end, temp, g, ans, visited);
                if(found) return;
                temp = back;
                ans /= it->second;
            }
        }
    }
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_map<string, vector<pair<string, double>>> g;
        // build the graph
        for(int i = 0; i < equations.size(); i++){
            g[equations[i][0]].push_back(make_pair(equations[i][1], 1/values[i]));
            g[equations[i][1]].push_back(make_pair(equations[i][0], values[i]));
        }
        vector<double> ans(queries.size());
        for(int i = 0; i < queries.size(); i++){
            string start = queries[i][1];
            string end = queries[i][0];
            // if the start node is not in the graph
            if(g.found(start) == g.end() || g.found(end) == g.end())
                ans[i] = -1.0;
            else if(start == end)
                ans[i] = 1.0;
            else{
                unordered_set<string> visited;
                visited.insert(start);
                ans[i] = 1.0;
                found = false;
                dfs(start, end, start, g, ans[i], visited);
                if(!found)
                    ans[i] = -1.0;
            }
        }
        return ans;
    }
};
```

