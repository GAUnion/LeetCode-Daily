# 997. Find the Town Judge

## Problem Description

In a town, there are `N` people labelled from `1` to `N`. There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

1. The town judge trusts nobody.
2. Everybody (except for the town judge) trusts the town judge.
3. There is exactly one person that satisfies properties 1 and 2.

You are given `trust`, an array of pairs `trust[i] = [a, b]` representing that the person labelled `a` trusts the person labelled `b`.

If the town judge exists and can be identified, return the label of the town judge. Otherwise, return `-1`.

## Solutions

1. Record the frequency of trust and trusted. Check two vectors to find trustother[i] == 0 && trusted[i] == N - 1

```
int findJudge(int N, vector<vector<int>>& trust) {
        vector<int> trustother(N+1), trusted(N+1);
        
        for(auto item:trust)
        {
            trustother[item[0]]++;
            trusted[item[1]]++;
        }
        
        for(int i = 1; i <= N; i++)
        {
            if(trustother[i] == 0 && trusted[i] == N - 1)
                return i;
        }
        
        return -1;
        
    }
```

2. Consider trust as a graph, all pairs are directed edge. The judge point should be the one with in-degree - out-degree = N - 1

```
int findJudge(int N, vector<vector<int>>& trust) {
        vector<int> count(N + 1);
        for (auto& t : trust)
            count[t[0]]--, count[t[1]]++;
        for (int i = 1; i <= N; ++i) {
            if (count[i] == N - 1) return i;
        }
        return -1;
    }
```

