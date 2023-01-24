---
title: LeetCode Weekly Contest 120
date: 2019-01-20 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Jan 20th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 977. Squares of a Sorted Array

```c++
class Solution {
public:
    vector<int> sortedSquares(vector<int>& a) {
        for (int &x: a) x = x * x;
        sort(a.begin(), a.end());
        return a;
    }
};
```



### 978. Longest Turbulent Subarray

WA了一次...中间结果超出maxint了...zz

```c++
class Solution {
public:
    int maxTurbulenceSize(vector<int>& a) {
        if (a.size() == 1) return 1;
        
        vector<int> b;
        for (int i = 0; i < a.size() - 1; ++i) b.push_back(a[i] - a[i + 1]);
        
        int ret = 1;
        int cur = 1;
        for (int i = 1; i < b.size(); i++) {
            if (b[i] > 0 && b[i - 1] < 0 || b[i] < 0 && b[i - 1] > 0) {
                cur++;
                ret = max(ret, cur);
            }
            else {
                cur = 1;
            }
        }
        
        return ret + 1;
    }
};
```



### 979. Distribute Coins in Binary Tree

还是要用递归的思路。

从下往上，依次算根节点和它左右子节点的差值，然后更新根节点的数值，递归即可。

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
    int solve(TreeNode* &root) {
        int t = 0;
        if (root -> left) {
            t += solve(root -> left);
        }
        if (root -> right) {
            t += solve(root -> right);
        } 
        
        
        if (root -> left) {
            int temp = root -> left -> val - 1;
            root -> val += temp;
            t += abs(temp);
        }
        if (root -> right) {
            int temp = root -> right -> val - 1;
            root -> val += temp;
            t += abs(temp);
        }
        
        return t;
    } 
    
    int distributeCoins(TreeNode* root) {
        int ret = solve(root);
        return ret;
    }
};
```



### 980. Unique Paths III

在二维地图上，从起点走到终点，必须经过所有非障碍物的点，问有多少种走法。

这题hard，但是通过率很高…看起来有某种简单的解法...

棋盘面积<=20，是不是可以暴力dfs...

```c++
int ret = 0;

class Solution {
public:
    void dfs(int x, int y, vector<vector<int>>& grid, int cur_zeros, int zeros) {
        if (grid[x][y] == 2) {
            if (cur_zeros == zeros) {
                ret ++;
            }
            return;
        }
                
        if (x - 1 >= 0 && grid[x - 1][y] != -1) {
            if (!grid[x - 1][y]) {
                grid[x - 1][y]--;
                dfs(x - 1, y, grid, cur_zeros + 1, zeros);
                grid[x - 1][y]++;
            }
            else dfs(x - 1, y, grid, cur_zeros, zeros);
        }
        if (x + 1 < grid.size() && grid[x + 1][y] != -1) {
            if (!grid[x + 1][y]) {
                grid[x + 1][y]--;
                dfs(x + 1, y, grid, cur_zeros + 1, zeros);
                grid[x + 1][y]++;
            }
            else dfs(x + 1, y, grid, cur_zeros, zeros);
        }
        if (y - 1 >= 0 && grid[x][y - 1] != -1) {
            if (!grid[x][y - 1]) {
                grid[x][y - 1]--;
                dfs(x, y - 1, grid, cur_zeros + 1, zeros);
                grid[x][y - 1]++;
            }
            else dfs(x, y - 1, grid, cur_zeros, zeros);
        }
        if (y + 1 < grid[0].size() && grid[x][y + 1] != -1) {
            if (!grid[x][y + 1]) {
                grid[x][y + 1]--;
                dfs(x, y + 1, grid, cur_zeros + 1, zeros);
                grid[x][y + 1]++;
            }
            else dfs(x, y + 1, grid, cur_zeros, zeros);
        }
        
    }
    
    int uniquePathsIII(vector<vector<int>>& grid) {
        ret = 0;
        int zeros = 0;
        int startx, starty, endx, endy;
        int m = grid.size();
        int n = grid[0].size();
        for (int i = 0; i < m; ++i) 
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) zeros++;
                if (grid[i][j] == 1) {
                    startx = i;
                    starty = j;
                }
                if (grid[i][j] == 2) {
                    endx = i;
                    endy = j;
                }
            }
        
        grid[startx][starty] = -1;
        dfs(startx, starty, grid, 0, zeros);
        
        return ret;
    }
};
```



pseudo AK......



