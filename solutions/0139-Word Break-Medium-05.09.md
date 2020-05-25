### Leetcode 139 Word Break

The understanding of the problem is to find the substrings of s to recover the string s. The substrings do have intersection part and do not miss any word in s. Meeting these requirements, we can have other duplicate words in the remaining substrings.

Let's look at some specific cases.

"leetcode"
["leet","code"]

"leetcode"
["leet","e","cod"]

"leetcode"
["e","cod","leet"]

"leetcode"
["lee","code","t","lee"]

"leetcode"
["leetc","code","de","code","lee","t"]

The results of these examples are all true.

"leetcode"

["leetc","cod","de"]

"leetcode"

["leet","ode"]

The results of the two examples are both false.

**Solution: Dynamic Programming**

dp is an array that contains booleans.

dp[i] is True if there is a word in the dictionary that ends at ith index of s AND d is also True at the beginning of the word.

Example:

s = "leetcode"

words = ["leet", "code"]

dp[3] is True because there is "leet" in the dictionary that ends at 3rd index of "leetcode".

dp[7] is True because there is "code" in the dictionary that ends at the 7th index of "leetcode" AND dp[3] is True.

The result is the last index of d.

```java
public boolean wordBreak32(String s, List<String> wordDict) { //7ms
    boolean[] dp = new boolean[s.length() + 1];
    Set<String> set = new HashSet<>(wordDict);

    dp[0] = true;
    for (int i = 0; i < s.length(); i++) {
        for (int j = i + 1; j <= s.length(); j++) {
            if(dp[i] && set.contains(s.substring(i, j)))
                dp[j] = true;
        }
    }
    return dp[s.length()];
}
```

To optimize the solution, we could consider the length of words in wordDict.

```java
public boolean wordBreak4(String s, List<String> wordDict) { // 3ms
    int n = s.length();
    boolean[] dp = new boolean[n + 1];

    dp[0] = true;

    for (int i = 0; i < n; i++) {
        if (!dp[i]) continue;

        for (String word : wordDict){
            int j = i + word.length();
            if(j <= n && s.substring(i, j).equals(word))
                dp[j] = true;
        }
    }
    return dp[n];
}
```

