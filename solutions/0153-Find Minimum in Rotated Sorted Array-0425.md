# 153. Find Minimum in Rotated Sorted Array

## Problem Description

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

You may assume no duplicate exists in the array.

## Solutions

***
### 1. traversal

```c++
int findMin(vector<int>& nums) {
        for (int i = 0; i < nums.size()-1;i++)
        {
            if(nums[i]>nums[i+1])
                return nums[i + 1];
        }
        return nums[0];
    }
```

***
### 2. binary search

1. When nums [mid]> nums [right] indicates the increasing area in the left half of mid, indicating that the smallest element is in the area between mid and end<br />
2. When nums [mid]<= nums [right] indicates the increasing area in the right half of mid, indicating that the smallest element is in the area between start and mid<br />

```c++
int findMin(vector<int>& nums) {
        if(nums.size()==1||nums[0]<nums.back())
            return nums[0];
        int l = 0, h = nums.size() - 1;
        while (l<h)
        {
            int m = (l + h) / 2;
            if(nums[m]>=nums[0])
            {
                l = m + 1;
            }
            else
            {
                h = m;
            }
        }
        return nums[h];
    }
```

