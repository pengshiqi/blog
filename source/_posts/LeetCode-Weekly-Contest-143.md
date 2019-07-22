---
title: LeetCode Weekly Contest 143
date: 2019-06-30 12:04:21
tags: LeetCode
categories: LeetCode
---



Time: Jun 30th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



第3题在12:04分才通过…可惜了...

<img src="143.png" style="zoom:100%" />



### 1103. Distribute Candies to People

```c++
class Solution {
public:
    vector<int> distributeCandies(int candies, int n) {
        vector<int> ret(n, 0);
        int cur = 1, idx = 0;
        while (candies) {
            if (idx == n) idx = 0;
            if (candies <= cur) {
                ret[idx] += candies;
                break;
            }
            ret[idx] += cur;
            candies -= cur;
            cur++; 
            idx++; 
        }
        return ret;
    }
};
```



### 1104. Path In Zigzag Labelled Binary Tree

```c++
class Solution {
public:
    vector<int> pathInZigZagTree(int label) {
        int level = (int)log2(label);
        
        vector<int> ret;
        
        int x;
        x = label - (1 << level);
        if (level & 1) {
            x = (1 << level) - 1 - x;
        } 
        
        for (; level >= 0; --level) {
            if (level & 1) {
                ret.push_back((1 << level) + (1 << level) - 1 - x);   
            } else {
                ret.push_back((1 << level) + x);   
            }
            x /= 2;
        }
        
        reverse(ret.begin(), ret.end());
        return ret;
    }
};
```



### 1105. Filling Bookcase Shelves

DP题...

边界条件没搞对，debug了好久...

```python
class Solution:
    def minHeightShelves(self, books: List[List[int]], L: int) -> int:
        n = len(books)
        cost = [999999999] * n
        T = [x[0] for x in books]
        H = [x[1] for x in books]
                
        idx = n - 1
        cur_l = 0
        cur_max = 0
        while cur_l + T[idx] <= L and idx >= 0:
            cur_l += T[idx]
            cur_max = max(cur_max, H[idx])
            cost[idx] = cur_max
            idx -= 1
        
        for i in range(n - 2, -1, -1):
            cur = T[i]
            for j in range(i + 1, n):
                if cur <= L and max(H[i: j]) + cost[j] <= cost[i]:
                    cost[i] = max(H[i: j]) + cost[j]
                    
                cur += T[j]
        
        return cost[0]
```



### 1106. Parsing A Boolean Expression

递归字符串处理。

```python
class Solution:
    def _split(self, s: str) -> List[str]:
        if len(s) == 0:
            return []
        
        idx = 0
        cur = ""
        cnt_l = 0
        ret = []
        
        while idx < len(s):
            if s[idx] == "," and cnt_l == 0:
                ret.append(cur)
                cur = ""
            else:
                cur += s[idx]
                if s[idx] == "(":
                    cnt_l += 1
                if s[idx] == ")":
                    cnt_l -= 1
            idx += 1
        ret.append(cur)
        
        return ret
        
    
    def parseBoolExpr(self, s: str) -> bool:
        if s == "f": return False
        if s == "t": return True
        
        if s[0] == "!":
            return not self.parseBoolExpr(s[2:-1])
        
        if s[0] == "&":
            s = s[2:-1]
            l = self._split(s)
            f = True
            for x in l:
                f = f and self.parseBoolExpr(x)
                if not f:
                    return False;
            return True
        
        if s[0] == "|":
            s = s[2:-1]
            l = self._split(s)
            f = False
            for x in l:
                f = f or self.parseBoolExpr(x)
                if f:
                    return True;
            return False
```





