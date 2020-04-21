# 526.Beautiful Arrangement

## Problem

Suppose you have **N** integers from 1 to N. We define a beautiful arrangement as an array that is constructed by these **N** numbers successfully if one of the following is true for the ith position (1 <= i <= N) in this array:

1. The number at the ith position is divisible by **i**.
2. **i** is divisible by the number at the ith position.

Now given N, how many beautiful arrangements can you construct?

## Solutions

### 1. Backtracking + visited

Start iteration from back to front.

Use the visited array to identify whether the number i has been used.

```c++
class Solution {
public:
    int countArrangement(int N){
        count = 0;
        visited.resize(N);
        for (int i = 0; i < N; i++) visited[i] = false;
        dfs(N, N);
        return count;
    }
    
private:
    vector<bool> visited;
    int count;
    void dfs(int N, int nextPos){
        if (nextPos == 0){
            count++;
            return;
        }
        for (int i = 1; i <= N; i++){
            if (visited[i - 1] || nextPos % i != 0 && i % nextPos != 0)
                continue;
            visited[i - 1] = true;
            dfs(N, nextPos - 1);
            visited[i - 1] = false;
        }
    } 
};
```

Time Complexity: O(N^2)

Space Complexity: O(N)

### 2. Backtracking + swap

Solution 1 is slow because in each `dfs` it needs to iterate through all the numbers. This solution is more efficient, we uses the swap function instead of the visited array. Also from the back to the front,  The main ideas are as follows:

1. Initialize v, the placed element is [1 ... N] (excluding 0)
2. Assuming that the elements from p + 1 to N-1 have been placed, consider the position of p
3. `for (int i = 0; i <p; i ++)` traverses the number from v [0] to v [p-1], if v [i] meets the condition, then swap v [i] and v [p-1] 
4. When recursing again, the only thing to consider is how to place the index from 0 to p-1
5. When returning the result, exchange v [i] and v [p-1] back

```c++
class Solution {
public:
    int countArrangement(int N) {
        vector<int> v;
        v.resize(N);
        for (int i = 0; i < N; i++) v[i] = i + 1;
        return count(N, v);
    }

private:
    int count(int n, vector<int>& v){
        if (n <= 1) return 1;
        int res = 0;
        for (int i = 0; i < n; i++){
            if (v[i] % n == 0 || n % v[i] == 0){
                swap(v[i], v[n - 1]);
                res += count(n - 1, v);
                swap(v[i], v[n - 1]);
            }
        }
        return res;
    }
};
```

Time Complexity: O(N^2)

Space Complexity: O(N)