# Course Schedule Ⅱ

### Method: Binary Search

Because the array is sorted, we can use the binary search method. In order to find the position, we can turn to find the first number which is no smaller than target. In this way, whether this number is bigger than target or equal to target, the position is certain. But there is a special situation that all the numbers in the array are smaller than target. In this situation, the position we get is the last number in the array. We can easily return the position after that to solve it.

We use two pointers to do the binary search. When the pointers are equal, we get the position. Here is the code in C++:

```c++
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        int mid;
        while(left != right){
            mid = (left + right) / 2;
            if(nums[mid] < target) left = mid + 1;
            else right = mid;
        }
        if(nums[left] < target) return left + 1;
        else return left;
    }
};
```
