---
title: LeetCode Weekly Contest 114
date: 2018-12-09 10:02:21
tags: LeetCode
categories: LeetCode
---



## LeetCode Weekly Contest 114

Time: Dec 09th, 2018 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### [Verifying an Alien Dictionary](https://leetcode.com/contest/weekly-contest-114/problems/verifying-an-alien-dictionary)

第一题根据题目给的字符顺序，建个索引来比较大小就可以了。

```c++
class Solution {
public:
    bool larger(string a, string b, map<char, int> m) {
        for (int i = 0; i < min(a.length(), b.length()); i++) {
            if (m[a[i]] < m[b[i]]) return false;
            else if (m[a[i]] > m[b[i]]) return true;
        }
        
        if (a.length() > b.length()) return true;
        
        return false;
    }
    
    bool isAlienSorted(vector<string>& words, string order) {
        map<char, int> m;
        for (int i = 0; i < order.length(); i++) m[order[i]] = i;
        
        if (words.size() == 1) return true;
        
        for (int i = 0; i < words.size() - 1; i++) {
            if (larger(words[i], words[i + 1], m)) return false;
        }
        
        return true;
    }
};
```



### [Array of Doubled Pairs](https://leetcode.com/contest/weekly-contest-114/problems/array-of-doubled-pairs)

第二题先将A排序，然后分为负数和非负数两部分处理。

对于每个数x，查找是否有x*2，如果找不到，那么直接return false，找到后将这两个数都标记为用过。

如果所有数都能找到/被用过，那么return true。

```c++
class Solution {
public:
    bool canReorderDoubled(vector<int>& A) {
        sort(A.begin(), A.end());
        
        vector<int> negative;
        vector<int> positive;
        
        map<int, int> neg_map;
        map<int, int> pos_map;
        
        for (auto x: A) {
            if (x < 0) {
                negative.push_back(x);
                if (neg_map.count(x)) neg_map[x] += 1;
                else neg_map[x] = 1;
            }
            else {
                positive.push_back(x);
                if (pos_map.count(x)) pos_map[x] += 1;
                else pos_map[x] = 1;
            }
        } 
        
        if (negative.size() % 2) return false;
        
        // negative      
        for (int i = 0; i < negative.size(); i++) {
            if (neg_map[negative[i]] == 0) continue;
            neg_map[negative[i]] -= 1;
            
            bool flag = false;
            
            for (int j = i + 1; j < negative.size(); j++) {
                if (negative[j] * 2 == negative[i] && neg_map[negative[j]] > 0) {
                    flag = true;
                    neg_map[negative[j]] -= 1;
                    break;
                }
            }
            
            if (!flag) return false; 
        }
             
        // positive
        for (int i = 0; i < positive.size(); i++) {
            if (pos_map[positive[i]] == 0) continue;
            pos_map[positive[i]] -= 1;
            
            bool flag = false;
            
            for (int j = i + 1; j < positive.size(); j++) {
                if (positive[j] == positive[i] * 2 && pos_map[positive[j]] > 0) {
                    flag = true;
                    pos_map[positive[j]] -= 1;
                    break;
                }
            }
            
            if (!flag) return false; 
        }
            
        return true;
    }
};
```



### [Delete Columns to Make Sorted II](https://leetcode.com/problems/delete-columns-to-make-sorted-ii/)

第三题开始自己想了个贪心算法…但是贼麻烦...实现起来很多细节都容易出错，耗费了大量时间。

最后还是参考了solution，别人的实现时真的简单...

```python
class Solution:
    def minDeletionSize(self, A):
        """
        :type A: List[str]
        :rtype: int
        """
        
        def is_sorted(x):
            return all([x[i] <= x[i + 1] for i in range(len(x) - 1)])
        
        ans = 0
        
        cur = [""] * len(A)
        
        for col in zip(*A):
            cur2 = cur[:]  # deep copy a list
            
            for i, letter in enumerate(col):
                cur2[i] += letter
            
            if is_sorted(cur2):
                cur = cur2
            else:
                ans += 1
        
        return ans 
```



### [Tallest Billboard](https://leetcode.com/problems/tallest-billboard/)

这个dp有点难...状态转移方程有点巧妙…参考了solution

另外，这里使用了 `lru_cache` 来做记忆化。

```python
from functools import lru_cache
class Solution:
    def tallestBillboard(self, rods):
        """
        :type rods: List[int]
        :rtype: int
        """
        
        @lru_cache(None)
        def dp(i, s):
            if i == len(rods):
                return 0 if s == 0 else float('-inf')
            return max(dp(i + 1, s), dp(i + 1, s - rods[i]), rods[i] + dp(i + 1, s + rods[i]))
        
        return dp(0, 0)
```





