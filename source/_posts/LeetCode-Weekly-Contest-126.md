---
title: LeetCode Weekly Contest 126
date: 2019-03-03 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Mar 3rd, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 1002. Find Common Characters

collections.Counter 是个好东西

```python
class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        if len(A) == 0:
            return []
        
        from collections import Counter
        d = Counter(A[0])
        
        for i in range(1, len(A)):
            cur = Counter(A[i])
            for k, v in d.items():
                d[k] = min(d[k], cur.get(k, 0))
        
        ret = []
        for k, v in d.items():
            ret += [k] * v
        return ret
```



### 1003. Check If Word Is Valid After Substitutions

递归题。

```python
class Solution:
    def isValid(self, S: str) -> bool:
        if len(S) == 0:
            return True
        
        flag = False
        new_S = ""
        
        i = 0
        while i < len(S):
            if S[i: i + 3] == "abc":
                i += 3
                flag = True
                continue
            else:
                new_S += S[i]
                i += 1
        
        if not flag:
            return False
        
        if len(new_S) == 0:
            return True
        
        return self.isValid(new_S)
```



### 1004. Max Consecutive Ones III

记录每个0出现的位置。

```python
class Solution:
    def longestOnes(self, a: List[int], k: int) -> int:
        zero_pos = []
        f = []
        
        for i in range(len(a)):
            if a[i] == 0:
                zero_pos.append(i)
                
            
            if len(zero_pos) <= k:
                f.append(i + 1)
            else:
                f.append(i - zero_pos[- k - 1])
                    
        ret = 0
        for x in f:
            ret = max(ret, x)
        return ret
```



### 1000. Minimum Cost to Merge Stones 

这是个9分题...

先写了个贪心，果然不太对… 还是要dp才行...

合并石子的升级版，原问题是每次合并两堆，改成了每次合并K堆。

```c++

```





