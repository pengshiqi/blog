---
title: LeetCode Weekly Contest 137
date: 2019-05-19 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: May 19th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



一晃两个月过去了…好久没做比赛了...

前两个月做了不少公司的实习笔试题，整体来说偏简单了，还是LWC比较有意思。



### 1046. Last Stone Weight

维护一个排序数组就可以了。

因为数据范围很小，所以时间复杂度比较高也能通过。

```c++
class Solution {
public:
    int lastStoneWeight(vector<int>& a) {
        sort(a.begin(), a.end());
        while (a.size() >= 2) {
            int last = a[a.size() - 1] - a[a.size() - 2];
            if (!last) {
                a.pop_back();
                a.pop_back();
            }
            else {
                a.pop_back();
                a.pop_back();
                a.push_back(last);
                sort(a.begin(), a.end());
            }
        }
        
        if (a.size() == 1) return a[0];
        else return a[1] - a[0];
    }
};
```



### 1047. Remove All Adjacent Duplicates In String

这题是维护一个stack。

```c++
class Solution {
public:
    string removeDuplicates(string S) {
        stack<char> s;
        for (auto &x: S) {
            if (s.empty()) s.push(x);
            else {
                if (s.top() == x) s.pop();
                else s.push(x);
            }
        }
        
        string ret = "";
        while (!s.empty()) {
            ret += s.top();
            s.pop();
        }
        reverse(ret.begin(), ret.end());
        
        return ret;
    }
};
```



### 1048. Longest String Chain 

这题还是有点意思的。

一开始是当做最长上升子序列问题来做，时间复杂度O(n^2)。

然后WA了，发现题目并没有要求保证word chain的顺序与原顺序相同。

再想想，如果word1是word2的predecessor的话，那么word1的长度一定比word2少1，所以将words按长度排序后，就是一个最长上升子序列问题了。

PS：最后一个test case很恶心，1000个长度为16的字符串...额外加了一个判断条件才AC...

```python
from functools import lru_cache

class Solution:
    
    @lru_cache(None)
    def pre(self, a, b):
        if not len(a) + 1 == len(b):
            return False
        idx = 0
        while idx < len(a) and a[idx] == b[idx]:
            idx += 1
        if a[idx:] == b[idx + 1:]:
            return True
        return False
    
    def longestStrChain(self, words: List[str]) -> int:
        n = len(words)
        dp = [1] * n
        ret = 1
        
        words = sorted(words, key=lambda x: len(x))
        
        if len(set([len(x) for x in words])) == 1:
            return 1
        
        for i in range(1, n):
            for j in range(i):
                if self.pre(words[j], words[i]):
                    dp[i] = max(dp[i], dp[j] + 1)
            ret = max(ret, dp[i]);
                
        return ret
```



### 1049. Last Stone Weight II

这题和第一题的区别在于：可以选任意two rocks。这样就从一个模拟题变成了一个dp题。

这题没有太多想法，去参考了 lee215 的解答。

基本的想法是：将这些数分为两组，求两组数之和的最小差值。这就是一个简单的背包问题了。

(这个问题转换就非常巧妙…使用集合可以很轻松地完成这些操作...)

```python
class Solution:
    def lastStoneWeightII(self, stones: List[int]) -> int:
        dp, s = {0}, sum(stones)
        for stone in stones:
            dp = {x + stone for x in dp} | {x - stone for x in dp}
        return min([abs(x) for x in dp])
```





