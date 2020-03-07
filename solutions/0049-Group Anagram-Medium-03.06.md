#Group Anagram

**Problem:** Given an array of strings, group anagrams together.

**Example:**
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
- - -
#### Solution 1
For each string, compare whether the number of occurrences of each character is equal. If they are equal, put them in a list as a category.
Solution 1 is a general solution, whatever the string contains (e.g. uppercase letters, lowercase letters, numbers) can be solved with this algorithm.
```java
public List<List<String>> groupAnagrams(String[] strs) {
    List<List<String>> res = new ArrayList<>();
    boolean[] used = new boolean[strs.length];
    for (int i = 0; i < strs.length; i++) {
        List<String> temp = null;
        if (!used[i]) {
            temp = new ArrayList<String>();
            temp.add(strs[i]);
            //Compare each pair to see if the strings match.
            for (int j = i + 1; j < strs.length; j++) {
                if (equals(strs[i], strs[j])) {
                    used[j] = true;
                    temp.add(strs[j]);
                }
            }
        }
        if (temp != null) {
            res.add(temp);
        }
    }
    return res;
}

private boolean equals(String string1, String string2) {
    Map<Character, Integer> hash = new HashMap<>();
    //Record the number of occurrences of each character in the first string, and accumulate.
    for (int i = 0; i < string1.length(); i++) {
        if (hash.containsKey(string1.charAt(i))) {
            hash.put(string1.charAt(i), hash.get(string1.charAt(i)) + 1);
        } else {
            hash.put(string1.charAt(i), 1);
        }
    }
   //Record the number of occurrences of each character in the second string, and subtract from the previous numbers.
    for (int i = 0; i < string2.length(); i++) {
        if (hash.containsKey(string2.charAt(i))) {
            hash.put(string2.charAt(i), hash.get(string2.charAt(i)) - 1);
        } else {
            return false;
        }
    }
    //Determine whether the number of times for each character is 0 or not. None zero returns false directly.
    Set<Character> set = hash.keySet();
    for (char c : set) {
        if (hash.get(c) != 0) {
            return false;
        }
    }
    return true;
}
```
#### Solution 2
Sort each string alphabetically, and uniquely map a class of strings to the same position.
```java
public List<List<String>> groupAnagrams(String[] strs){
    List<List<String>> res =  new ArrayList<>();
    if (strs == null || strs.length == 0) return res;
    HashMap<String,Integer> map= new HashMap<>();
    for (String str : strs) {
    	char[] c = str.toCharArray();
    	Arrays.sort(c);
    	String s = new String(c);
    	if(map.containsKey(s)) {
    		List<String> list = res.get(map.get(s));
    		list.add(str);
    	} else {
    		List<String> list = new ArrayList<>();
    		list.add(str);
    		map.put(s, res.size());
    		res.add(list);
    	}
    }
    return res;
}
```
#### Solution 3
In order to avoid sorting and reduce time complexity, record the number of occurrences of each character of the string to complete the mapping.
 ```java
HashMap<String, List<String>> map = new HashMap<>();
    for(String str : strs) {
    	int[] count = new int[26];
    	for(Character c : str.toCharArray()) {
    		count[c-'a']++;
    	}
    	String s = "";
    	for (int i = 0; i < count.length; i++) {
			//The keys can be set arbitrarily but in order, even without the letters.
            s += String.valueOf(count[i]) + String.valueOf((char) i + 'a');
		}
    	if (map.containsKey(s)) {
    		List<String> list = map.get(s);
    		list.add(str);
    	} else {
    		List<String> list = new ArrayList<>();
    		list.add(str);
    		map.put(s, list);
    	}
    }
    return new ArrayList<>(map.values());
}
```






