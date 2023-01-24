---
title: LeetCode Weekly Contest 140
date: 2019-06-09 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Jun 09th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



Exciting~第一次排名进top100！

<img src="140.png" style="zoom:100%" />

这一次全用python写的，写起来快了不少...



### 5083. Occurrences After Bigram

送分题。

```python
class Solution:
    def findOcurrences(self, text: str, first: str, second: str) -> List[str]:
        l = text.split()
        ret = []
        for i in range(len(l) - 2):
            if l[i] == first and l[i + 1] == second:
                ret.append(l[i + 2])
        return ret
```



### 5087. Letter Tile Possibilities

tiles的长度最长也就是7，dfs就可以了，使用set来去重。

```python
class Solution:
    ans = set()
    
    def dfs(self, s: str, cur: str):
        self.ans.add(cur)
        for i in range(len(s)):
            self.dfs(s[:i] + s[i + 1:], cur + s[i])
    
    def numTilePossibilities(self, tiles: str) -> int:
        self.ans = set()
        for i in range(len(tiles)):
            self.dfs(tiles[:i] + tiles[i + 1:], tiles[i])
        return len(self.ans)
```



### 5084. Insufficient Nodes in Root to Leaf Paths

这题我用了两个函数，第一个是计算每一条path的值，并用 lru_cache 保存下来，第二个是修改树的结构。

还是那个基本套路，关于二叉树的题，都可以通过递归来解决。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from functools import lru_cache

class Solution:
    
    @lru_cache(None)
    def cal_sum(self, root, cur, limit):
        
        if not root:
            return (cur >= limit)
                
        return self.cal_sum(root.left, cur + root.val, limit) or self.cal_sum(root.right, cur + root.val, limit)
        
        
    def solve(self, root, cur, limit):
        if not root:
            return None
        
        if not self.cal_sum(root.left, cur + root.val, limit):
            root.left = None
        else:
            root.left = self.solve(root.left, cur + root.val, limit)
            
        if not self.cal_sum(root.right, cur + root.val, limit):
            root.right = None
        else:
            root.right = self.solve(root.right, cur + root.val, limit)
        
        return root
        
    
    def sufficientSubset(self, root: TreeNode, limit: int) -> TreeNode:
        if self.cal_sum(root, 0, limit):
            return self.solve(root, 0, limit)
        else:
            return None
```



### 5086. Smallest Subsequence of Distinct Characters

这题我做了有点久，一开始TLE了。

首先只有26个字母，字符串长度最长为1000，我们可以把每个字母出现的位置都记录下来。

然后使用贪心，因为要字典序最小，所以要尽量让字典序小的字母出现在前面。

但是选择每个字母的时候，要考虑其他的字母在该字母之后还需要出现过，用一个all()函数判断。

选择每个字母的位置的时候，使用了bisect，内置的二分查找。

```python
import bisect

class Solution:

    def smallestSubsequence(self, text: str) -> str:
        d = {x: [] for x in set(text)}
        for i in range(len(text)):
            d[text[i]].append(i)
        order = sorted(list(d.keys()))
        check = {x:False for x in order}

        ans = ""
        cur = -1
        for i in range(len(order)):
            for x in order:
                if not check[x]:
                    pos = bisect.bisect(d[x], cur)
                    if pos == len(d[x]):
                        continue
                    new_cur = d[x][pos]
                    if all([d[y][-1] >= new_cur for y in order if not check[y]]):
                        break

            ans += x
            cur = new_cur
            check[x] = True
                
        return ans
```





