---
title: LeetCode Weekly Contest 141
date: 2019-06-16 12:04:21
tags: LeetCode
categories: LeetCode
---



Time: Jun 16th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



上周重新评测了…本来75名的，后来掉到了160多…果然还是菜...

这周罚时了20min…因为罚时掉了100多名...

<img src="141.png" style="zoom:100%" />



### 1089. Duplicate Zeros

打卡题。

```python
class Solution:
    def duplicateZeros(self, arr: List[int]) -> None:
        """
        Do not return anything, modify arr in-place instead.
        """
        t = [x for x in arr]
        index1 = 0
        index2 = 0
        while index2 < len(arr):
            if t[index1] == 0:
                arr[index2] = 0
                if index2 + 1 >= len(arr):
                    return
                arr[index2 + 1] = 0
                index2 += 2
            else:
                arr[index2] = t[index1]
                index2 += 1
            index1 += 1
```



### 1090. Largest Values From Labels

排序题。

```python
from collections import defaultdict

class Solution:
    def largestValsFromLabels(self, values: List[int], labels: List[int], num_wanted: int, use_limit: int) -> int:
        bundle = zip(values, labels)
        bundle = sorted(bundle, key=lambda x: x[0], reverse=True)
        
        d = defaultdict(int)
        ret = 0
        
        for x in bundle:
            if num_wanted == 0:
                break
            if d[x[1]] == use_limit:
                continue
            ret += x[0]
            d[x[1]] += 1
            num_wanted -= 1
            
        return ret        
```



### 1091. Shortest Path in Binary Matrix

有障碍物的最短路径。

这是个BFS题，我居然WA了三次。。。

只能说，太久不写BFS，生疏了。

而且，偷懒用了很多python的特性来实现queue…这样并不好...

```python
class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        # BFS
        n = len(grid)
        
        if grid[0][0] == 1:
            return -1
        
        q = [(0, 0)]
        step = [1]
        
        f = {}
        for i in range(n):
            for j in range(n):
                f[(i, j)] = False
        f[(0, 0)] = True
        
        while (n - 1, n - 1) not in q:
            if len(q) == 0:
                return -1
            
            x, y = q[0]
            cur_step = step[0]
            
            if x + 1 < n:
                if y + 1 < n and grid[x + 1][y + 1] == 0 and not f[(x + 1, y + 1)]:
                    q.append((x + 1, y + 1))
                    step.append(cur_step + 1)
                    f[(x + 1, y + 1)] = True
                if grid[x + 1][y] == 0 and not f[(x + 1, y)]:
                    q.append((x + 1, y))
                    step.append(cur_step + 1)
                    f[(x + 1, y)] = True
                if y - 1 >= 0 and grid[x + 1][y - 1] == 0 and not f[(x + 1, y - 1)]:
                    q.append((x + 1, y - 1))
                    step.append(cur_step + 1)
                    f[(x + 1, y - 1)] = True
            if x - 1 >= 0:
                if y + 1 < n and grid[x - 1][y + 1] == 0 and not f[(x - 1, y + 1)]:
                    q.append((x - 1, y + 1))
                    step.append(cur_step + 1)
                    f[(x - 1, y + 1)] = True
                if grid[x - 1][y] == 0 and not f[(x - 1, y)]:
                    q.append((x - 1, y))
                    step.append(cur_step + 1)
                    f[(x - 1, y)] = True
                if y - 1 >= 0 and grid[x - 1][y - 1] == 0 and not f[(x - 1, y - 1)]:
                    q.append((x - 1, y - 1))
                    step.append(cur_step + 1)
                    f[(x - 1, y - 1)] = True
            if y + 1 < n and grid[x][y + 1] == 0 and not f[(x, y + 1)]:
                q.append((x, y + 1))
                step.append(cur_step + 1)
                f[(x, y + 1)] = True
            if y - 1 >= 0 and grid[x][y - 1] == 0 and not f[(x, y - 1)]:
                q.append((x, y - 1))
                step.append(cur_step + 1)
                f[(x, y - 1)] = True
            
            q = q[1:]
            step = step[1:]
                    
        return step[q.index((n - 1, n - 1))]
```



### 1092. Shortest Common Supersequence

最短公共超序列，其实等价于求最长公共子序列。

不仅要求长度，而且要把序列记录下来，hard中的简单题了。。。

```python
class Solution:
    def shortestCommonSupersequence(self, str1: str, str2: str) -> str:
        # 等价于最长公共子序列
        l1 = len(str1)
        l2 = len(str2)
        f = [[0 for _ in range(l2 + 1)] for _ in range(l1 + 1)]
        d = [[None for _ in range(l2 + 1)] for _ in range(l1 + 1)]
        for i in range(l1):
            for j in range(l2):
                if str1[i] == str2[j]:
                    f[i + 1][j + 1] = f[i][j] + 1
                    d[i + 1][j + 1] = "ok"
                elif f[i][j + 1] > f[i + 1][j]:
                    f[i + 1][j + 1] = f[i][j + 1]
                    d[i + 1][j + 1] = "up"
                else:
                    f[i + 1][j + 1] = f[i + 1][j]
                    d[i + 1][j + 1] = "left"
                
        s = []
        while f[l1][l2]:
            c = d[l1][l2]
            if c == 'ok':   #匹配成功，插入该字符，并向左上角找下一个
                s.append(str1[l1 - 1])
                l1 -= 1
                l2 -= 1 
            if c =='left':  #根据标记，向左找下一个
                l2 -= 1
            if c == 'up':   #根据标记，向上找下一个
                l1 -= 1
        s.reverse()
        s = "".join(s)
                
        ret = ""
        idx1 = 0
        idx2 = 0
        p = 0
        
        while p < len(s):
            while str1[idx1] != s[p]:
                ret += str1[idx1]
                idx1 += 1
            while str2[idx2] != s[p]:
                ret += str2[idx2]
                idx2 += 1
            ret += s[p]
            p += 1
            idx1 += 1
            idx2 += 1
        
        ret += str1[idx1:]
        ret += str2[idx2:]
            
        return ret
```





