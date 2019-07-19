---
title: LeetCode Weekly Contest 117
date: 2018-12-30 10:24:21
tags: LeetCode
categories: LeetCode
---



Time: Dec 30th, 2018 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 965. Univalued Binary Tree

用一个队列来遍历这棵树就好了。

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
    bool isUnivalTree(TreeNode* root) {
        if (!root) return true;
        
        int val = -1;
        
        queue<TreeNode*> s;
        s.push(root);
        
        TreeNode* cur;
        
        while (!s.empty()) {
            cur = s.front();
            
            if (val < 0) val = cur -> val;
            
            if (cur -> val != val) return false;
            
            if (cur -> left) s.push(cur -> left);
            if (cur -> right) s.push(cur -> right);
            
            s.pop();
        }
        
        return true;
        
    }
};
```



### 966. Numbers With Same Consecutive Differences

其实就是一个dfs枚举所有情况...

WA了一次，是因为没考虑K==0的情况，导致last+K和last-K重复了...

```C++
class Solution {
public:
    void dfs(int N, int K, string cur, vector<int>& ans) {
        int m = cur.size();
        if (m == N) {
            ans.push_back(stoi(cur));
            return;
        }
        
        int last = cur[m - 1] - '0';
        if (K != 0) {
            if (last + K < 10) dfs(N, K, cur + to_string(last + K), ans);
            if (last - K >= 0) dfs(N, K, cur + to_string(last - K), ans);
        }
        else {
            if (last < 10) dfs(N, K, cur + to_string(last), ans);
        }
        
    }
    
    vector<int> numsSameConsecDiff(int N, int K) {
        
        vector<int> ans;
        
        if (N == 1) return {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        
        for (int i = 1; i < 10; i++) {
            dfs(N, K, to_string(i), ans);
        }
        
        return ans;
    }
};
```



### 967. Vowel Spellchecker

第一次直接暴力匹配果然TLE了...

然后就考虑先建一个set，然后建一个map，来简化查找过程。

其中，为了考虑元音，把set中的word每一个元音都替换为'a'。

```c++
class Solution {
public:
    
    bool isEqual(string s1, string s2) {
        if (s1.size() != s2.size()) return false;
        for (int i = 0; i < s1.size(); i++) {
            if (s1[i] == s2[i] || abs(s1[i] - s2[i]) == 32) continue;
            return false;
        }
        return true;
    }
    
    string vowel_to_a(string s) {
        set<char> vowel = {'a', 'e', 'i', 'o', 'u'};
        string ret = "";
        for (int i = 0; i < s.size(); i++) {
            if (vowel.find(s[i]) != vowel.end()) ret += 'a';
            else ret += s[i];
        }
        return ret;
    }
    
    vector<string> spellchecker(vector<string>& wordlist, vector<string>& queries) {
        
        vector<string> ans;
       
        set<string> s;
        map<string, vector<string>> m;
        string t;
            
        for (auto x: wordlist) {
            t = x;
            transform(t.begin(), t.end(), t.begin(), ::tolower);
            t = vowel_to_a(t);
            if (s.find(t) != s.end()) {
                m[t].push_back(x);
            }
            else {
                s.insert(t);
                m[t] = {x};
            }
        }
        
        bool flag;
        for (auto query: queries) {
            t = query;
            transform(t.begin(), t.end(), t.begin(), ::tolower);
            t = vowel_to_a(t);
            if (s.find(t) != s.end()) {
                vector<string> cand = m[t];
                flag = false;
                // case 1
                for (int i = 0; i < cand.size(); i++) 
                    if (cand[i] == query) {
                        flag = true;
                        ans.push_back(cand[i]);
                        break;
                    }
                if (flag) continue;
                else {
                    // case 2
                    flag = false;
                    for (int i = 0; i < cand.size(); i++) 
                        if (isEqual(cand[i], query)) {
                            flag = true;
                            ans.push_back(cand[i]);
                            break;
                        }
                    if (flag) continue;
                    else {
                        //case 3
                        ans.push_back(cand[0]);
                    }
                }   
            }
            else ans.push_back("");
        }
        
        return ans;
    }
};
```



### 968. Binary Tree Cameras

感觉应该是树形DP...

```python

```





