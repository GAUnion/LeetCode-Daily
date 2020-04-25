### [LeetCode 39](https://leetcode.com/problems/combination-sum/) Combination Sum

##### Solution 1 Backtracking

The backtracking idea is based on

1. Choose
2. Explore
3. Un-choose the last number in the list

Enumerate all the situations forward, and go back when you get a solution or you are not in the right way.

Note: in this problem, we allow duplicate numbers in the solutions, so we return with the number i, not i+1.

```java
public class {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
    	List<List<Integer>> res = new ArrayList<List<Integer>>();
    	helper(candidates, target, 0, res, new ArrayList<>());
    	return res;
    }
    private void helper(int[] candidates, int target, int index, List<List<Integer>> res, ArrayList<Integer> list) {
    	if (target < 0) return;
    	else if (target == 0) { 
    		res.add(new ArrayList<>(list));
    	} else {
    		for (int i = index; i < candidates.length; i++) {
        		if (candidates[i] > target) continue;
        		list.add(candidates[i]);
        		helper(candidates, target- candidates[i], i, res, list);
                //backtrack i, not i + 1
        		list.remove(list.size() - 1);
        	}
    	}
    }
}
```

##### Solution 2 Dynamic Programming

We use a list called dp and then find dp [0], dp [1] ... dp [target] in turn.

dp[0] represents a combination of all cases where the sum is 0。

dp[1] represents a combination of all cases where the sum is 1。

dp[2] represents a combination of all cases where the sum is 2。

...

dp[target] represents the combination of all cases of target, which is also the answer to the problem.



We use two layers of 'for loop', the outer layer traverses candidates, and the inner layer traverses dp.

For the example, candidates = [ 2, 3, 6, 7 ] , target = 7 

Consider candidates [ 0 ]，get dp [ 0 ], get dp [ 1 ],  ... get dp [ 7 ];

consider candidates [ 1 ]，get dp [ 0 ], get dp [ 1 ],  ... get dp [ 7 ];

consider  candidates [ 2 ]，get dp [ 0 ], get dp [ 1 ],  ... get dp [ 7 ];

consider candidates [ 3 ]，get dp [ 0 ], get dp [ 1 ],  ... get dp [ 7 ].

Each loop will update dp [ 7 ] once, and the last updated dp [ 7 ] is what we want.

```java
class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
    	// sort candidates to try them in asc order
        Arrays.sort(candidates); 
        // dp[t] stores all combinations that add up to t
        List<List<Integer>>[] dp = new ArrayList[target + 1];
        
        // build up dp
        for(int t = 0; t <= target; t++) {
            dp[t] = new ArrayList();
            List<List<Integer>> res = new ArrayList();
            
            // for each t, find possible combinations
            for(int j = 0; j < candidates.length && candidates[j] <= t; j++) {
                if(candidates[j] == t) {
                    // itself can form a list
                    res.add(Arrays.asList(candidates[j]));  
                } else {
                    for(List<Integer> prevlist: dp[t-candidates[j]]) { 
             		// only add to list when the candidates[j] >= the last element, so the list remains ascending order, can prevent duplicate (ex. [2 3 3], not [3 2 3]); equal is needed since we can choose the same element many times   
                        if(candidates[j] >= prevlist.get(prevlist.size()-1)){
                            List temp = new ArrayList(prevlist); 
                            // temp is needed since 
                            temp.add(candidates[j]); 
                            // cannot edit prevlist inside 4eeach looop
                            res.add(temp);
                        }
                    }
                }
            }
            dp[t] = res;
        }
        return dp[target];
    }
}
```

