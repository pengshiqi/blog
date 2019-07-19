---
title: LeetCode Weekly Contest 125
date: 2019-02-24 12:04:21
tags: LeetCode
categories: LeetCode
---



Time: Feb 24th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 997. Find the Town Judge

总觉得我做的比较蠢...

维护两个集合的列表，分别表示第i个人trust谁和第i个人被谁trust。

```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        trusted_list = [set() for x in range(n + 1)]
        trust_list = [set() for x in range(n + 1)]
        
        for x in trust:
            trusted_list[x[1]].add(x[0])
            trust_list[x[0]].add(x[1])
        
        ret = []
        for i in range(1, n + 1):
            if len(trusted_list[i]) == n - 1 and len(trust_list[i]) == 0:
                ret.append(i)
        
        if len(ret) == 1:
            return ret[0]
        return -1
```



### 998. Maximum Binary Tree II

先deconstruct，再construct，都能通过递归实现。

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def deconstruct(self, root: TreeNode) -> List[int]:
        l = [] if root.left is None else self.deconstruct(root.left)
        r = [] if root.right is None else self.deconstruct(root.right)
        return l + [root.val] + r
    
    def construct(self, nums: List[int]) -> TreeNode:
        if not nums:
            return None

        i = nums.index(max(nums))

        node = TreeNode(nums[i])

        node.left = self.construct(nums[:i])
        node.right = self.construct(nums[i + 1:])

        return node
    
    def insertIntoMaxTree(self, root: TreeNode, val: int) -> TreeNode:
        A = self.deconstruct(root)
        B = A + [val]
        return self.construct(B)
```



### 999. Available Captures for Rook

四个方向判断一下就好了...

```c++
class Solution {
public:
    int numRookCaptures(vector<vector<char>>& board) {
        int rx, ry;
        for (int i = 0; i < 8; ++i)
            for (int j = 0; j < 8; ++j)
                if (board[i][j] == 'R')
                {
                    rx = i;
                    ry = j;
                    break;
                }
        
        int ret = 0;
        
        // N
        for (int i = rx - 1; i >= 0; --i) {
            if (board[i][ry] == '.')
                continue;
            else if (board[i][ry] == 'B')
                break;
            else if (board[i][ry] == 'p') {
                ret++;
                break;
            }
        }
        
        // S
        for (int i = rx + 1; i < 8; ++i) {
            if (board[i][ry] == '.')
                continue;
            else if (board[i][ry] == 'B')
                break;
            else if (board[i][ry] == 'p') {
                ret++;
                break;
            }
        }
        
        // W
        for (int i = ry - 1; i >= 0; --i) {
            if (board[rx][i] == '.')
                continue;
            else if (board[rx][i] == 'B')
                break;
            else if (board[rx][i] == 'p') {
                ret++;
                break;
            }
        }

        // E
        for (int i = ry + 1; i < 8; ++i) {
            if (board[rx][i] == '.')
                continue;
            else if (board[rx][i] == 'B')
                break;
            else if (board[rx][i] == 'p') {
                ret++;
                break;
            }
        }
        
        return ret;
    }
};
```



### 1001. Grid Illumination

像这种看起来两层循环会超时，一般都是以空间换时间，多用一些哈希的数据结构（比如dict，unordered_map）就可以了。

```python
class Solution:
    def gridIllumination(self, N: int, lamps: List[List[int]], queries: List[List[int]]) -> List[int]:
        row = {}
        column = {}
        diag1 = {}
        diag2 = {}
        
        lamp_dict = {}
        
        for [x, y] in lamps:
            if x in row.keys():
                row[x] += 1
            else:
                row[x] = 1
            if y in column.keys():
                column[y] += 1
            else:
                column[y] = 1
            if x - y in diag1.keys():
                diag1[x - y] += 1
            else:
                diag1[x - y] = 1
            if x + y in diag2.keys():
                diag2[x + y] += 1
            else:
                diag2[x + y] = 1
            lamp_dict[N * (x) + y] = 1
            
        ret = []
        for [x, y] in queries:
            if row.get(x, 0) or column.get(y, 0) or diag1.get(x - y, 0) or diag2.get(x + y, 0):
                ret.append(1)
            else:
                ret.append(0)
                
            # turn off [x, y] adjacents
            # row x
            if lamp_dict.get(N * (x) + y, 0):
                lamp_dict[N * (x) + y] = 0
                row[x] -= 1
                column[y] -= 1
                diag1[x - y] -= 1
                diag2[x + y] -= 1
            
            if y - 1 >= 0 and lamp_dict.get(N * (x) + y - 1, 0):
                lamp_dict[N * (x) + y - 1] = 0
                row[x] -= 1
                column[y - 1] -= 1
                diag1[x - (y - 1)] -= 1
                diag2[x + (y - 1)] -= 1
            
            if y + 1 < N and lamp_dict.get(N * (x) + y + 1, 0):
                lamp_dict[N * (x) + y + 1] = 0
                row[x] -= 1
                column[y + 1] -= 1
                diag1[x - (y + 1)] -= 1
                diag2[x + (y + 1)] -= 1
           
            # row x - 1
            if N * (x - 1) + y >= 0 and lamp_dict.get(N * (x - 1) + y, 0):
                lamp_dict[N * (x - 1) + y] = 0
                row[x - 1] -= 1
                column[y] -= 1
                diag1[x - 1 - y] -= 1
                diag2[x - 1 + y] -= 1
            
            if y - 1 >= 0 and lamp_dict.get(N * (x - 1) + y - 1, 0):
                lamp_dict[N * (x - 1) + y - 1] = 0
                row[x - 1] -= 1
                column[y - 1] -= 1
                diag1[x - 1 - (y - 1)] -= 1
                diag2[x - 1 + (y - 1)] -= 1
            
            if y + 1 < N and lamp_dict.get(N * (x - 1) + y + 1, 0):
                lamp_dict[N * (x - 1) + y + 1] = 0
                row[x - 1] -= 1
                column[y + 1] -= 1
                diag1[x - 1 - (y + 1)] -= 1
                diag2[x - 1 + (y + 1)] -= 1
              
            # row x + 1  
            if N * (x + 1) + y < N * N and lamp_dict.get(N * (x + 1) + y, 0):
                lamp_dict[N * (x + 1) + y] = 0
                row[x + 1] -= 1
                column[y] -= 1
                diag1[x + 1 - y] -= 1
                diag2[x + 1 + y] -= 1
            
            if y - 1 >= 0 and lamp_dict.get(N * (x + 1) + y - 1, 0):
                lamp_dict[N * (x + 1) + y - 1] = 0
                row[x + 1] -= 1
                column[y - 1] -= 1
                diag1[x + 1 - (y - 1)] -= 1
                diag2[x + 1 + (y - 1)] -= 1
            
            if y + 1 < N and lamp_dict.get(N * (x + 1) + y + 1, 0):
                lamp_dict[N * (x + 1) + y + 1] = 0
                row[x + 1] -= 1
                column[y + 1] -= 1
                diag1[x + 1 - (y + 1)] -= 1
                diag2[x + 1 + (y + 1)] -= 1
        
        return ret
        
```





