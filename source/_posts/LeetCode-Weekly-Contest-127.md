---
title: LeetCode Weekly Contest 127
date: 2019-03-10 12:04:21
tags: LeetCode
categories: LeetCode
---



Time: Mar 10th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



### 1005. Maximize Sum Of Array After K Negations

```c++
class Solution {
public:
    int largestSumAfterKNegations(vector<int>& a, int k) {
        sort(a.begin(), a.end());
        
        int idx = 0;
        while (k && idx < a.size()) {
            if (a[idx] < 0) {
                a[idx] = -a[idx];
                idx++;
                k--;
            }
            else break;
        }
        
        int ret = 0;
        if (!k) {
            for (auto x: a) ret += x;
        }
        else {
            sort(a.begin(), a.end());
            if (k % 2) {
                a[0] = -a[0];
            }
            for (auto x: a) ret += x;
        }
        
        return ret;
    }
};
```



### 1006. Clumsy Factorial

```c++
typedef long long int64;

class Solution {
public:
    int clumsy(int n) {
        vector<int64> a;
        
        int64 last = -n;
        int t;
        for (int i = n - 1; i >= 1; i--) {
            t = (n - i) % 4;
            if (t == 0) last = i;
            else if (t == 1) last *= i;
            else if (t == 2) {
                last /= i;
                a.push_back(-last);
                last = 0;
            }
            else if (t == 3) a.push_back(i);
        }
        
        a.push_back(-last);
        
        int ret = 0;
        for (auto x: a) ret += x;
        return ret;
    }
};
```



### 1007. Minimum Domino Rotations For Equal Row 

```c++
class Solution {
public:
    int minDominoRotations(vector<int>& a, vector<int>& b) {
        bool flag = false;
        vector<int> idx;
        
        for (int i = 1; i <= 6; ++i) {
            bool cur_f = true;
            for (int j = 0; j < a.size(); ++j) {
                if (a[j] == i || b[j] == i) continue;
                else {cur_f = false; break;}
            }            
            if (cur_f) {
                flag = true;
                idx.push_back(i);
            }
        }
        
        if (!flag) return -1;
        
        int ret = 1 << 20;
        for (auto &i: idx) {
            int not_a = 0, not_b = 0;
            for (int j = 0; j < a.size(); ++j) {
                if (a[j] != i) not_a++;
                if (b[j] != i) not_b++;
            }
            
            ret = min(ret, min(not_a, not_b));
        }
        
        return ret;
    }
};
```



### 1008. Construct Binary Search Tree from Preorder Traversal



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
    
    TreeNode* bstFromPreorder(vector<int>& preorder) {
        if (!preorder.size()) return NULL;
        
        TreeNode* root = new TreeNode(preorder[0]);
        
        vector<int> l, r;
        for (int i = 1; i < preorder.size(); ++i) {
            if (preorder[i] < preorder[0]) l.push_back(preorder[i]);
            else r.push_back(preorder[i]);
        }
        
        root -> left = bstFromPreorder(l);
        root -> right = bstFromPreorder(r);
        
        return root;
    }
};
```





