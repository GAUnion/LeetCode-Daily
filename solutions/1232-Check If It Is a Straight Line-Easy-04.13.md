# 1232. Check If It Is a Straight Line

## Problem Description

You are given an array `coordinates`, `coordinates[i] = [x, y]`, where `[x, y]` represents the coordinate of a point. Check if these points make a straight line in the XY plane.

## Solution

This is a simple math problem. Just make sure (y_i - y_0)*(x_1 - x_0) == (y_1 - y_0)*(x_i - x_0)



```
class Solution {
public:
    bool checkStraightLine(vector<vector<int>>& coordinates) {
        if(coordinates.size() <= 2)
            return true;
        
        int dx1 = coordinates[1][0] - coordinates[0][0];
        int dy1 = coordinates[1][1] - coordinates[0][1];
        
        for(int i = 2; i < coordinates.size(); i++)
        {
            if((coordinates[i][1] - coordinates[0][1])*dx1 != (coordinates[i][0] - coordinates[0][0])*dy1)
                return false;
        }
        
        return true;
    }
};
```

