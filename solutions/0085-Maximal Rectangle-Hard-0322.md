# 85. Maximal Rectangle

## Problem: 

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

**Example:**

````
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
````

## Solutions

### 1. Based on Largest Rectangle in Histogram

Maintain a temp array of the same size as the number of columns. Copy the first row to this temp array and find the largest rectangular area for the histogram. Then keep adding elements of next row to this temp array if they are not zero. If they are zero then put zero there. Every time calculate the max area in the histogram.

More explanation [on this video](https://www.youtube.com/watch?v=g8bSdXCG-lA).

````python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix or not matrix[0]: return 0
        rows, cols = len(matrix), len(matrix[0])
        height = [[0]*cols for _ in range(rows)]
        for i in range(rows):
            for j in range(cols):
                if matrix[i][j] == '1':
                    height[i][j] = height[i-1][j] + 1
        return max(self.largestRectangleArea(row) for row in height)

    def largestRectangleArea(self, heights):  #leetcode84
        heights.append(0)
        stack, maxarea = [], 0
        for i in range(len(heights)):
            while stack and heights[stack[-1]] > heights[i]:
                h = heights[stack.pop()]
                w = i if not stack else i-stack[-1]-1
                maxarea = max(area, h*w)
            stack.append(i)
        return maxarea     
````

 * Time complexity: O(M*N)
 * Space complexity: O(M)

### 2. Maximum Height of Each Point

This DP solution proceeds row by row. Let the maximal rectangle area at row i and column j be computed by [right(i, j) - left(i, j)] * height(i, j).

`left` means at current position, what is the index of left bound of the rectangle with `height[j]`. '0' means at this position, no rectangle. 
`right` means the right bound index of this rectangle. 'n' means no rectangle.                                           `height` means from top to this position, there are how many '1'

All the 3 variables `left`, `right`, and `height` can be determined by the information from the previous row and the current row. 

The code is as below. The loops can be combined for speed but here are separated for more clarity of the algorithm.

````python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix or not matrix[0]: return 0
        rows, cols = len(matrix), len(matrix[0])
        left = [0] * cols # initialize left as the leftmost boundary possible
        right = [cols] * cols # initialize right as the rightmost boundary possible
        height = [0] * cols
        maxarea = 0

        for i in range(rows):
            cur_left, cur_right = 0, cols
            # update height
            for j in range(cols):
                if matrix[i][j] == '1': height[j] += 1
                else: height[j] = 0
            # update left
            for j in range(cols):
                if matrix[i][j] == '1': left[j] = max(left[j], cur_left)
                else:
                    left[j] = 0
                    cur_left = j + 1
            # update right
            for j in range(cols-1, -1, -1):
                if matrix[i][j] == '1': right[j] = min(right[j], cur_right)
                else:
                    right[j] = cols
                    cur_right = j
            # update the area
            for j in range(cols):
                maxarea = max(maxarea, height[j] * (right[j] - left[j]))
        return maxarea
````

 * Time complexity: O(M*N)
 * Space complexity: O(M)

### 3. Bitwise Operations 

In python, a loop is slow but bitwise operations are fast. The following solution utilizes the bitwise operation and takes a brute-force-like approach to solve the problem. Code is from this [discussion](https://leetcode-cn.com/problems/maximal-rectangle/solution/jing-miao-jie-fa-wei-yun-suan-by-zhu-meng-10/).

````python
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        if not matrix or not matrix[0]: return 0
        # Treat each line as a binary number and then convert it to an integer
        nums = [int(''.join(row), base=2) for row in matrix]
        ans, N = 0, len(nums)
    
        for i in range(N):
            j, num = i, nums[i]
            while j < N:
            #After the AND operation, num is converted to 1 in binary, which means that from 							i to j, the columns that can form a rectangle
                num = num & nums[j]
                if not num:
                    break
                l, curnum = 0, num
            #Calculate curnum and the number shifted by one bit to the left
                while curnum:
                    l += 1
                    curnum = curnum & (curnum << 1)
                ans = max(ans, l * (j - i + 1))
                j += 1
        return ans
````

