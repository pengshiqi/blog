---
title: LeetCode Weekly Contest 115
date: 2018-12-17 21:17:21
tags: LeetCode
categories:
- 学习
---



## LeetCode Weekly Contest 115

Time: Dec 16th, 2018 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### [Check Completeness of a Binary Tree](https://leetcode.com/contest/weekly-contest-115/problems/check-completeness-of-a-binary-tree/)

从上往下，一层层check就行了。

看了下cui aoxiang的代码，这种二叉树的问题，用递归是一种比较好的思路。

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
    bool isEmpty(vector<TreeNode*>& curLevel) {
        for (int i = 0; i < curLevel.size(); i++) {
            if (curLevel[i]) return false;
        }
        return true;
    }
    
    bool isCompleteTree(TreeNode* root) {
        if (!root) return true;
        
        vector<TreeNode*> curLevel = {root};
        
        while (!isEmpty(curLevel)) {
            vector<TreeNode*> nextLevel;
            nextLevel.clear();
            for (int i = 0; i < curLevel.size(); i++) {
                TreeNode* curNode = curLevel[i];
                if (!curNode) {
                    for (int j = i + 1; j < curLevel.size(); j++) {
                        if (curLevel[j]) return false;
                    }
                    if (!isEmpty(nextLevel)) return false;
                    return true;
                }
                nextLevel.push_back(curNode->left);
                nextLevel.push_back(curNode->right);
            }
            curLevel = nextLevel;
        }
        return true;
    }
};
```



### [Prison Cells After N Days](https://leetcode.com/contest/weekly-contest-115/problems/prison-cells-after-n-days/)

因为只有8个cell，$2^8=256$，所以这一定会出现循环...

其实可以不必使用vector的map，八个二进制的数可以换算成十进制，也就0-256，使用位运算可以简化操作。

```C++
class Solution {
public:
    vector<int> prisonAfterNDays(vector<int>& cells, int N) {
        map<vector<int>, int> m;
        map<int, vector<int>> m2;
        m.clear();
        m2.clear();
        
        if (!N) return cells;
        
        m[cells] = 0;
        m2[0] = cells;
        
        int day = 1;
        int loop_start, loop_end;
        vector<int> last = cells;
        while (1) {
            vector<int> cur(8);
            cur[0] = 0;
            cur[7] = 0;
            for (int i = 1; i < 7; i++) {
                if (last[i - 1] - last[i + 1] == 0) 
                    cur[i] = 1;
                else cur[i] = 0;
            }
            if (m.count(cur)) {
                loop_start = m[cur];
                loop_end = day;
                break;
            }
            m[cur] = day;
            m2[day] = cur;
            last = cur;
            day++;
        }
        
        if (N < loop_end) return m2[N];
        
        int mod = (N - loop_start) % (loop_end - loop_start);
        
        return m2[mod + loop_start];
    }
};
```



### [Regions Cut By Slashes](https://leetcode.com/contest/weekly-contest-115/problems/regions-cut-by-slashes/)

先构图，然后求连通区域的数量。

每个cell可以划分成四个子cell，使用并查集来求连通区域个数。

```c++

```





### [Delete Columns to Make Sorted III](https://leetcode.com/contest/weekly-contest-115/problems/delete-columns-to-make-sorted-iii/)

GG 思密达… II都做不来，III根本不想做...

好像也就是一个最大上升子序列的问题？？

```python

```





