# 42. Trapping Rain Water

### Description

Given *n* non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

### Solution 1: Dynamic Programming

We first iterate to find the highest bar from the right and from the left at each position. Then we can determine the height of water at each position.

1. Find maximum height of bar from the left/right end upto an index i in the array left_max/right_max
2. Iterate over the height array and add each trap to the sum: 
           ans += min(left_max[i], right_max[i]) - height[i]

```c++
int trap(vector<int>& height)
{
	if(height == null)
		return 0;
    int ans = 0;
    int size = height.size();
    vector<int> left_max(size), right_max(size);
    left_max[0] = height[0];
    for (int i = 1; i < size; i++) {
        left_max[i] = max(height[i], left_max[i - 1]);
    }
    right_max[size - 1] = height[size - 1];
    for (int i = size - 2; i >= 0; i--) {
        right_max[i] = max(height[i], right_max[i + 1]);
    }
    for (int i = 1; i < size - 1; i++) {
        ans += min(left_max[i], right_max[i]) - height[i];
    }
    return ans;
}
```

### Solution 2: 2 Pointers

Based on the DP solution, we may think of some way to do it iteratively to save the space of two arrays. Notice that as soon as we find a bar at the left side higher than a bar at the right side, the trap height is determined by the lower one. So we can maintain left_max and right_max at the position during the iteration using 2 pointers.

1. Assign two pointer left=0, right=size-1
2. While left<right, iterate the array and find the smaller one of height[left] and height[right]. If it is larger than the corresponding left_max/right_max, update it. Else add the trap height to the answer. 

```c++
int trap(vector<int>& height)
{
    int left = 0, right = height.size() - 1;
    int ans = 0;
    int left_max = 0, right_max = 0;
    while (left < right) {
        if (height[left] < height[right]) {
            height[left] >= left_max ? (left_max = height[left]) : ans += (left_max - height[left]);
            ++left;
        }
        else {
            height[right] >= right_max ? (right_max = height[right]) : ans += (right_max - height[right]);
            --right;
        }
    }
    return ans;
}
```

