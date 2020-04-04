# 289 - Game of Life

### Description 

According to the [Wikipedia's article](https://en.wikipedia.org/wiki/Conway's_Game_of_Life): "The **Game of Life**, also known simply as **Life**, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a *board* with *m* by *n* cells, each cell has an initial state *live* (1) or *dead* (0). Each cell interacts with its [eight neighbors](https://en.wikipedia.org/wiki/Moore_neighborhood) (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

1. Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2. Any live cell with two or three live neighbors lives on to the next generation.
3. Any live cell with more than three live neighbors dies, as if by over-population..
4. Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.

Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

**Example:**

```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```

**Follow up**:

1. Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
2. In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?

### Solution 1: In-Place solution

This is the solution given in [Discussion](https://leetcode.com/problems/game-of-life/discuss/73230/C%2B%2B-O(1)-space-O(mn)-time): C++ O(1) space, O(mn) time. In my view, this solution is so genius and hard to be though of.

Since the board has ints but only the 1-bit is used, I use the 2-bit to store the new state. At the end, replace the old state with the new state by shifting all values one bit to the right.

**My C++ implementation**

```C++
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        if (board.empty()) return;
        int m = board.size(), n = board[0].size();
        for (int i=0; i<m; i++){
            for (int j=0; j<n; j++){
                int count = 0;
                for (int I=max(i-1,0); I<min(i+2,m); I++)
                    for (int J=max(j-1,0); J<min(j+2, n); J++)
                        count += board[I][J] & 1;
                if (count == 3 || count - board[i][j] == 3){
                    board[i][j] |= 2;
                }
            }
        }
        for (int i=0; i<m; i++){
            for (int j=0; j<n; j++)
                board[i][j] >>= 1;
        }
    }
};
```

**Explanation**

Note that the above `count` counts the live ones among a cell's neighbors and the cell itself. Starting with `int count = -board[i][j]` counts only the live neighbors and allows the neat test.

```
if ((count | board[i][j]) == 3)
```

### Solution 2: Infinite Board Solution

Also thanks to [discussion](https://leetcode.com/problems/game-of-life/discuss/73217/Infinite-board-solution), we have this solution.

**My Personal Explanation** 

The basic idea is to regard the board is infinite. Items on the border is regarded as surrounded by virtual nodes of 0 beyond border. Then we use the infinite board to count and return the final results.

**Author's Python Solution** (I add several notes for better understanding)

For the second follow-up question, here's a solution for an infinite board. Instead of a two-dimensional array of ones and zeros, I represent the board as a set of live cell coordinates.

```python
def gameOfLifeInfinite(self, live):
	# count the the coordinates that surrouned by 1
    ctr = collections.Counter((I, J)
                              for i, j in live
                              for I in range(i-1, i+2)
                              for J in range(j-1, j+2)
                              if I != i or J != j)
    # only returns the qualified coordinates (see the rules)
    return {ij
            for ij in ctr
            if ctr[ij] == 3 or ctr[ij] == 2 and ij in live}
```

And here's a wrapper that uses the above infinite board solution to solve the problem we have here at the OJ (submitted together, this gets accepted):

```python
def gameOfLife(self, board):
	# Record the live ones before
    live = {(i, j) for i, row in enumerate(board) for j, live in enumerate(row) if live}
    # Return the live ones after a change
    live = self.gameOfLifeInfinite(live)
    # Generate new board
    for i, row in enumerate(board):
        for j in range(len(row)):
            row[j] = int((i, j) in live)
```