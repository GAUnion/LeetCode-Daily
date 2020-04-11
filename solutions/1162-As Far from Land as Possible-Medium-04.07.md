# 1162. As Far from Land as Possible

### Problem:

Given an N x N `grid` containing only values `0` and `1`, where `0` represents water and `1` represents land, find a water cell such that its distance to the nearest land cell is maximized and return the distance.

The distance used in this problem is the *Manhattan distance*: the distance between two cells `(x0, y0)` and `(x1, y1)` is `|x0 - x1| + |y0 - y1|`.

If no land or water exists in the grid, return `-1`.

### Solution:

It's a typical 'Breadth First Search' problem and we chose a pretty clean solution.

We first built the unit data structure, then we built the BFS queue and do BFS on this graph.

```java
	private class Point {
        int x;
        int y;
        int time;
        public Point(int x, int y, int time) {
            this.x = x;
            this.y = y;
            this.time = time;
        }
    }
    public int maxDistance(int[][] grid) {
        Queue<Point> q = new ArrayDeque<>();
        int max = -1;
        boolean visited[][] = new boolean[grid.length][grid[0].length];
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    q.add(new Point(i, j, 0));
                }
            }
        }
        if (q.size() == grid.length * grid[0].length) {
            return -1;
        }
        while (!q.isEmpty()) {
            Point p = q.poll();
            if (p.x < 0 || p.x > grid.length - 1 || p.y < 0 || p.y > grid[0].length - 1)
                continue;
            if (visited[p.x][p.y])
                continue;
            
            visited[p.x][p.y] = true;
            q.add(new Point(p.x + 1, p.y, p.time + 1));
            q.add(new Point(p.x - 1, p.y, p.time + 1));
            q.add(new Point(p.x, p.y + 1, p.time + 1));
            q.add(new Point(p.x, p.y - 1, p.time + 1));
            max = Math.max(max, p.time);
        }
        return max;
    }
```

