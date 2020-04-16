# Word Search

### Problem Description

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

- **Example:**

  ```
  board =
  [
    ['A','B','C','E'],
    ['S','F','C','S'],
    ['A','D','E','E']
  ]
  
  Given word = "ABCCED", return true.
  Given word = "SEE", return true.
  Given word = "ABCB", return false.
  ```

   

  **Constraints:**

  - `board` and `word` consists only of lowercase and uppercase English letters.
  - `1 <= board.length <= 200`
  - `1 <= board[i].length <= 200`
  - `1 <= word.length <= 10^3`

### Solution：DFS

Very classic DFS problem. The trick to change the visited node to '*' could save the consuming space.

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (dfs(board, i, j, word, 0)) {
                    return true;
                } 
            }
        }
        return false;
    }
    public boolean dfs(char[][] board, int x, int y, String word, int i) {
        if (i == word.length()) {
            return true;
        }
        if (x < 0 || y < 0 || x >= board.length || y >= board[0].length || word.charAt(i) != board[x][y]) {
            return false;
        }
        board[x][y] = '*';
        boolean existed = dfs(board, x - 1, y, word, i + 1) ||
                          dfs(board, x, y - 1, word, i + 1) ||
                          dfs(board, x + 1, y, word, i + 1) ||
                          dfs(board, x, y + 1, word, i + 1);
        board[x][y] = word.charAt(i);
        return existed;
    }
}
```

