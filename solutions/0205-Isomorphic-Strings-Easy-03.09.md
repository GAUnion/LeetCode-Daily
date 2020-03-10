# 205. Isomorphic Strings

## [Problem Description](https://leetcode.com/problems/isomorphic-strings/)

Given two strings **s** and **t**, determine if they are isomorphic.

Two strings are isomorphic if the characters in **s** can be replaced to get **t**.

All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character but a character may map to itself.

## Solutions

### 1. Hash table

The idea of this solution is that, we need to map every char in the string **s** to the corresponding char in the **t**. Take the testcase "egg" and "add" as an example, we need to map 'e' to 'a' and 'g' to 'd'. Notice that This mapping should be a one to one injection, so we can use a hash table to map **s** to **t**, and another one to map **t** to **s** reversely.

Then we traverse the chars of **s** and **t** on the same position, if the there is no mapping record, we construct a new mapping. However, if the mapping is wrong, we should return false.

```c++
class Solution {
public:
    bool isIsomorphic(string s, string t) {
        unordered_map<char,char> table;
        unordered_map<char,char> r_table;
        for(int i=0; i<s.length(); i++){
            auto it = table.find(s[i]);
            if(it==table.end()){
                if(r_table.find(t[i])!=r_table.end())    return false;
                else{
                    table[s[i]] = t[i];
                    r_table[t[i]] = s[i];
                }
            }
            else if(it->second!=t[i])   return false;
        }
        return true;
    }
};
```

### [2. Optimized Hash table](https://leetcode.com/problems/isomorphic-strings/discuss/57963/8ms-C%2B%2B-Solution-without-Hashmap)

The basic idea of this solution is the same as the former one. And this solution adopted two optimization methods.

1. Implement a simple hash table, since a char can be easily presented as an int and used as an index.
2. Use a unique int, in this case, the index, to bridge the mapping.
   The tables in solution 1 is like: a->b, a<-b, so we need to examine forwards and backwards. However,
   The tables in solution 2 is like: a->5, 5<-b, so we only need to examine once.

```c++
class Solution {
public:
	bool is Isomorphic(string s, string t){
        char map_s[128] = { 0 };
        char map_t[128] = { 0 };
        int len = s.size();
        for (int i = 0; i < len; ++i)
        {
            if (map_s[s[i]]!=map_t[t[i]]) return false;
            map_s[s[i]] = i+1;	//	Add 1 to avoid mapping to 0 (original value)
            map_t[t[i]] = i+1;
        }
        return true;
	}
}
```

