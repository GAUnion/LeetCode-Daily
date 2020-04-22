# Problem:

Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets.

# Solutions

### Method 1

Using **HashSet**:

It's natural to think about using hash-like methods to solve 'no-duplicate' problem. 

This method uses HashSet to solve the problem intuitively. 

```java
class Solution {
    List<List<Integer>> result;
    Set<List<Integer>> set = new HashSet<>();
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        result = new ArrayList<>();
        if(nums == null || nums.length == 0){
            result.add(new ArrayList<>());
            return result;
        }
        
        Arrays.sort(nums);        
        subsetsWithDupUtility(nums, new ArrayList<>(), 0);
        return result;
    }
    
    public void subsetsWithDupUtility(int[] nums, List<Integer> current, int index){
        if(!set.contains(current)){
            result.add(new ArrayList<>(current));
            set.add(current);                    
			}
        
        for(int i = index; i < nums.length; i++){
            current.add(nums[i]);
            subsetsWithDupUtility(nums, current, i + 1);
            current.remove(current.size() - 1);
        }
    }
}
```

### Method 2:

##### DFS (Backtracking) solution

```java
class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> resultSet = new ArrayList<>();
        List<Integer> subset = new ArrayList<>();
        Arrays.sort(nums); // need to sort the array first
		
        dfs(0, nums, subset, resultSet);
        return resultSet;
    }
    
    public void dfs(int index, int[] nums, List<Integer> subset, List<List<Integer>> resultSet) {
        /* add subset into resultSet */
        resultSet.add(new ArrayList<>(subset));
        
        for (int i = index; i < nums.length; i++) {
            /* skip duplicates, the idea is same as combination sum II,
			where duplicated sets are not allowed in candidates */
            if (i != index && nums[i] == nums[i - 1])
                continue;
            
            subset.add(nums[i]);
            dfs(i + 1, nums, subset, resultSet);
			// Backtracking step
            subset.remove(subset.size() - 1);
        }
    }
}
```

