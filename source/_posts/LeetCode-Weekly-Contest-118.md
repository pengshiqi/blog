---
title: LeetCode Weekly Contest 118
date: 2019-01-06 10:24:21
tags: LeetCode
categories: LeetCode
---



Time: Jan 6th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 969. Pancake Sorting

依次把当前最大的数放到最后一个位置就好了，比如例子中，先放4，再放3，再放2，最后放1。

每把一个最大的数放到最后需要 pancake flip 两次：第一次把它放到第一位，第二次把它放到当前最后的位置。

所以总共的交换次数为 2 * A.length。

```c++
class Solution {
public:
    bool ordered(vector<int> &a) {
        for (int i = a.size() - 1; i >= 0; --i)
            if (a[i] != i + 1) return false;
        return true;
    }
    
    vector<int> pancakeSort(vector<int>& a) {        
        vector<int> ret;
        
        int cur_max = a.size();
        vector<int>::iterator it;
        int dist;
        
        while (!ordered(a)) {
            it = find(a.begin(), a.end(), cur_max);
            if (it != a.begin() + cur_max - 1)  {
                dist = distance(a.begin(), it);
                ret.push_back(dist + 1);
                reverse(a.begin(), it + 1);
                reverse(a.begin(), a.begin() + cur_max);
                ret.push_back(cur_max);
            }
            --cur_max;
        }       
        
        return ret;
    }
};
```



### 970. Powerful Integers

先找出最大的指数，注意排除底数为1的情况。

然后用set去重。

```c++
class Solution {
public:
    vector<int> powerfulIntegers(int x, int y, int bound) {
        vector<int> ret;
        int maxi = 0, maxj = 0;
        
        if (x == 1) maxi = 1;
        else {
            while ((int)pow(x, maxi) <= bound) ++maxi;
        }
        if (y == 1) maxj = 1;
        else {
            while ((int)pow(y, maxj) <= bound) ++maxj;
        }
        
        for (int i = 0; i < maxi; ++i) 
            for (int j = 0; j < maxj; ++j) {
                if ((int)pow(x, i) + (int)pow(y, j) <= bound) ret.push_back((int)pow(x, i) + (int)pow(y, j));
            }
        
        set<int> st(ret.begin(), ret.end());
        ret.assign(st.begin(), st.end());
        
        return ret;
    }
};
```



### 971. Flip Binary Tree To Match Preorder Traversal

关于二叉树的题，大多都是用递归的方法来解决...

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
    int count(TreeNode* root) {
        if (!root) return 0;
        return 1 + count(root -> left) + count(root -> right);
    }
    
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        vector<int> ret;
        
        if (!root) return {};
        
        if (root -> val != voyage[0]) return {-1};
        
        if (root -> left) {
             if (root -> left -> val != voyage[1]) {
                 TreeNode* temp = root -> left;
                 root -> left = root -> right;
                 root -> right = temp;
                 ret.push_back(root -> val);
             }
        }
        
        int cl = count(root -> left), cr = count(root -> right);
                
        vector<int> temp1(cl), temp2(cr);
        copy(voyage.begin() + 1, voyage.begin() + 1 + cl, temp1.begin());
        copy(voyage.begin() + 1 + cl, voyage.begin() + 1 + cl + cr, temp2.begin());
        
        vector<int> t1 = flipMatchVoyage(root -> left, temp1);
        vector<int> t2 = flipMatchVoyage(root -> right, temp2);
        
        ret.insert(ret.end(), t1.begin(), t1.end());
        ret.insert(ret.end(), t2.begin(), t2.end());
        
        if (find(ret.begin(), ret.end(), -1) != ret.end()) return {-1};
        
        return ret;
    }
};
```



### 972. Equal Rational Numbers

把循环小数转换成浮点数就好啦！

```python
class Solution:
    
    def trans(self, s):
        par = s.find("(")
        if par  == -1:
            return float(s)
        else:
            x = float(s[:par])
            rpar = s.find(")")
            repeat = int(s[par + 1 : rpar])
            len_repeat = rpar - par - 1
            
            deno = pow(10, len_repeat)
            y = float(s[:par] + s[par + 1 : rpar])
            y = y * deno
            return (y - x) / (deno - 1)
    
    def isRationalEqual(self, S, T):
        """
        :type S: str
        :type T: str
        :rtype: bool
        """
        return abs(self.trans(S) - self.trans(T)) < 1e-9
```



第一次AK！！！

提前两分钟全部做完！开心!



