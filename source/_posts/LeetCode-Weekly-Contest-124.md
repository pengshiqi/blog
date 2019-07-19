---
title: LeetCode Weekly Contest 124
date: 2019-02-17 12:04:21
tags: LeetCode
categories: LeetCode
---



Time: Feb 17th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 993. Cousins in Binary Tree

开两个map，记录每个节点的深度和父节点。

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    void cal(TreeNode* cur, map<int, int> &m, map<int, int> &par) {
        if (!cur) return;
        if (cur -> left) {
            m[cur -> left -> val] = m[cur -> val] + 1;
            par[cur -> left -> val] = cur -> val;
            cal(cur -> left, m, par);
        }
        if (cur -> right) {
            m[cur -> right -> val] = m[cur -> val] + 1;
            par[cur -> right -> val] = cur -> val;
            cal(cur -> right, m, par);
        } 
    }
    
    bool isCousins(TreeNode* root, int x, int y) {
        map<int, int> m;
        m.clear();
        
        map<int, int> par;
        par.clear();
        
        m[root -> val] = 0;
        par[root -> val] = -1;
        cal(root, m, par);
        
        return (m[x] == m[y]) && (par[x] != par[y]); 
    }
};
```



### 994. Rotting Oranges

```c++
class Solution {
public:
    int countFresh(vector<vector<int>> grid) {
        int freshCount = 0;
        for (int i = 0; i < grid.size(); ++i) {
            for (int j = 0; j < grid[0].size(); ++j) {
                if (grid[i][j] == 1) freshCount++;
            }
        }
        return freshCount;
    }
    int orangesRotting(vector<vector<int>>& grid) {
        int ret = 0;
        while (true) {
            vector<vector<int>> nextGrid = grid;
            
            for (int i = 0; i < grid.size(); ++i) {
                for (int j = 0; j < grid[0].size(); ++j) {
                    if (grid[i][j] == 2) {
                        if (i - 1 >= 0 && grid[i - 1][j] == 1) nextGrid[i - 1][j] = 2;
                        if (j - 1 >= 0 && grid[i][j - 1] == 1) nextGrid[i][j - 1] = 2;
                        if (i + 1 < grid.size() && grid[i + 1][j] == 1) nextGrid[i + 1][j] = 2;
                        if (j + 1 < grid[0].size() && grid[i][j + 1] == 1) nextGrid[i][j + 1] = 2;
                    }
                }
            }
            
            if (countFresh(nextGrid) == countFresh(grid)) break;
            
            ret++;
            grid = nextGrid;
            if (countFresh(grid) == 0) break;
        }
        
        if (countFresh(grid) != 0) return -1;
        return ret;
    }
};
```



### 995. Minimum Number of K Consecutive Bit Flips

从前往后，每找到一个0就翻转一次，这个贪心是正确的。

```c++
class Solution {
public:
    int minKBitFlips(vector<int>& a, int K) {
        int ret = 0;
        
        int cur = 0;
        while (cur < a.size()) {
            if (a[cur] == 0) {
                ret++;
                for (int j = 0; j < K; j++) {
                    if (cur + j >= a.size()) return -1;
                    a[cur + j] = 1 - a[cur + j];
                }
            }
            cur++;
        }
        
        return ret;
    }
};
```



### 996. Number of Squareful Arrays

自己写了个复杂的C++ dfs，结果TLE，还把自己搞晕了...

后来看别人写的巨简单...

```python
class Solution:
    def numSquarefulPerms(self, A: 'List[int]') -> 'int':
        c = collections.Counter(A)
        cand = {i: {j for j in c if int(((i + j) ** 0.5)) ** 2 == i + j} for i in c}
        self.res = 0
        def dfs(x, left=len(A) - 1):
            c[x] -= 1
            if left == 0:
                self.res += 1
            for y in cand[x]:
                if c[y]:
                    dfs(y, left - 1)
            c[x] += 1
        for x in c:
            dfs(x)
        return self.res
```





