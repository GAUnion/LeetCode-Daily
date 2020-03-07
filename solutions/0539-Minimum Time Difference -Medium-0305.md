# Minimum Time Difference

**Problem**: Given a 24-hour(hour:minute) time list, find the minimum time difference between any two times in the time list and represent it with minutes. 

## Convert, sort and select the minimum one

Firstly, convert the hour:minute format times to minute only format times. If the new time is not in the list, add these times into a list. Otherwise, return 0. 

Then, sort the list. Sorting can make sure that the minimum time difference is bound to be one of the subtraction results of adjacent times in the list. 

Finally, do the subtraction between adjacent times in the list and find the minimum result.

Checking time repetition and sorting are the key of optimizing the codes. 

Python code

```python
class Solution:
    def findMinDifference(self, timePoints: List[str]) -> int:
        d = set()
        for c in timePoints:
            k = int(c[: 2]) * 60 + int(c[3: ])
            if k in d:  
                return 0
            d.add(k)
        d = sorted(d)
        d.append(d[0] + 1440) #make sure d[-1] can subtract accurately with d[0]
        return min(d[i] - d[i - 1] for i in range(1, len(d)))
```

Reached double 100% in python

## Bucket sort

1.Take 24 hours as 24 buckets and push minutes into the buckets accordingly.
2.Sort within every bucket and calculate the minimum time difference within the bucket.
3.Calculate the first element in the bucket with the last element in the previous bucket.
4.Don't forget to calculate the first non-empty bucket and the last non-empty bucket.

Not an excellent method, but an interesting method.

C++ Code

 ````python
class Solution {
public:
    void helper(vector<vector<int>> &buckets, int &h, int &ret){
        int tmp;
        for (int j=1; j<buckets[h].size(); j++){
            tmp=buckets[h][j]-buckets[h][j-1];
            ret=min(tmp, ret);
        }
    }
    int findMinDifference(vector<string>& timePoints) {
        /*timePoints={
            "01:39","10:26","21:43"
        };*/
        vector<vector<int>> buckets(24);
        int hour, minute;
        int ret=INT_MAX;
        int tmp;
        for (int i=0; i<timePoints.size(); i++){
            hour=stoi(timePoints[i].substr(0, 2));
            minute=stoi(timePoints[i].substr(3, 5));
            buckets[hour].push_back(minute);
        }

        int pre=-1;
        
        for (int i=0; i<24; i++){
            if (buckets[i].size()){
                sort(buckets[i].begin(), buckets[i].end());
                helper(buckets, i, ret);
                if (pre!=-1){
                    tmp=i*60+buckets[i][0]-pre*60-buckets[pre].back();
                    tmp=min(tmp, 1440-tmp);
                    ret=min(ret,tmp);
                }
                pre=i;
            }
        }

        int i=0;
        while (buckets[i].size()==0){
            i++;
        }
        if (pre>i){
                tmp=pre*60+buckets[pre].back()-i*60-buckets[i][0];
                tmp=min(tmp, 1440-tmp);
                ret=min(ret,tmp);
        }
        
        return ret;
    }
};
 ````
Reached 57.91% in time and 5.69% in memory lol.

