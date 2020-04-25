### Leetcode 229 Majority Element II

The problem is similar to 169: find all elements that appear more than [n/2] times. 

We have three ways to solve 169.  (Reference: [link](https://www.bilibili.com/video/av63293677?spm_id_from=333.788.b_765f64657363.1))

1. Use hashmap to store the time of appearance of each number. 

   time complexity O(N), space complexity O(N)

2. Sort the array and find the middle number.

   time complexity O(NlogN), space complexity O(1)

3. Boyer-Moore Majority Vote algorithm. (Algorithm explanation: [link](http://goo.gl/64Nams))

   In the array, we keep throwing one in Group A where the number appears more than n/2 times and another in Group B where the number appears less than n/2 times. Finally, the left numbers are all in Group A.

   time complexity O(N), space complexity O(1)

Now we come back to 229.

**Solution 1:** Hashmap

In the map, the key stores the element, and the value stores the number of occurrences of the element. If the number of occurrences equals n / 3 + 1, add it to the result.

```java
public List<Integer> majorityElement(int[] nums) {
    int n = nums.length;
    HashMap<Integer, Integer> map = new HashMap<>();
    List<Integer> res = new ArrayList<>();
    for (int i = 0; i < n; i++) {
        int count = map.getOrDefault(nums[i], 0);
        // The previous quantity is already n/3, the current quantity is n/3 + 1
        if (count == n / 3) {
            res.add(nums[i]);
        }
        //Can end early
        if (count == 2 * n / 3 || res.size() == 2) {
            return res;
        }
        map.put(nums[i], count + 1);

    }
    return res;
}
```

**Solution 2:** Boyer-Moore Majority Vote ([link](https://www.bilibili.com/video/BV1z4411Q77E?from=search&seid=15592133547274512388))

The requirement is finding the majority for more than ceiling of [n/3], the answer would be less than or equal to two numbers. We can modify the algorithm to maintain two counters for two majorities.

Consider 3 cases:

1. there are no elements that appears more than n/3 times, then whatever the algorithm got from 1st round wound be rejected in the second round. 
2. there are only one elements that appears more than n/3 times, after 1st round one of the candi-cate must be that appears more than n/3 times (other elements (<2n/3) could only pair out for  less than n/3 times), the other candicate is not necessarily be the second most frequent but it would be rejected in 2nd round.
3. there are two elements appears more than n/3 times, candicates would contain both of them. (other elements (<n/3) couldn't pair out any of the majorities.)

```java
public List<Integer> majorityElement(int[] nums) {
    	if (nums == null || nums.length == 0)
    		return new ArrayList<Integer>();
    	List<Integer> res = new ArrayList<>();
    	int len = nums.length;
    	int candidate1 = 0, candidate2 = 1, count1 = 0, count2 = 0;
    	for (int i = 0; i < len; i++) {
    		if (nums[i] == candidate1)
    			count1++;
    		else if (nums[i] == candidate2)
    			count2++;
    		else if (count1 == 0) {
    			candidate1 = nums[i];
    			count1 = 1;
    		} else if (count2 == 0) {
    			candidate2 = nums[i];
    			count2 = 1;
    		} else {
    			count1--;
    			count2--;
    		}
    	}
    	count1 = 0; 
    	count2 = 0;
    	for (int i = 0; i < nums.length; i++) {
    		if (nums[i] == candidate1) count1++;
    		else if (nums[i] == candidate2) count2++;
    	}
    	if (count1 > len / 3) res.add(candidate1); 
    	if (count2 > len / 3) res.add(candidate2); 
    	return res;
    }
```





