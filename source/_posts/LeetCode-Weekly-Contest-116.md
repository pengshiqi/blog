---
title: LeetCode Weekly Contest 116
date: 2018-12-23 10:24:21
tags: LeetCode
categories: 
- 学习
---



Time: Dec 23th, 2018 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 961. N-Repeated Element in Size 2N Array

一道2分题...

排序+扫一遍，O(N)时间复杂度...

```c++
class Solution {
public:
    int repeatedNTimes(vector<int>& A) {
        sort(A.begin(), A.end());
        
        int last = A[0];
        for (int i = 1; i < A.size(); i++) {
            if (A[i] == last) {
                return last;
            }
            else last = A[i];
        }
    }
};
```



### 962. Maximum Width Ramp

`A.length <= 50000`，需要O(N)或者O(NlogN)的算法。

emmm 自己做了一会儿没做出来，参考了评论区大佬们的做法...

```C++
class Solution {
public:
    int maxWidthRamp(vector<int>& A) {
        int ans = 0;
        int index = 99999999;
        map<int, vector<int>> m;
        
        for (int i = 0; i < A.size(); i++) {
            m[A[i]] = {};
        }
        for (int i = 0; i < A.size(); i++) {
            m[A[i]].push_back(i);
        }
        
        vector<int> B = A;
        sort(B.begin(), B.end());
        
        for (int i = 0; i < B.size(); i++) {
            ans = max(ans, m[B[i]][m[B[i]].size() - 1] - index);
            index = min(index, m[B[i]][0]);
        }
        
        return ans;
    }
};
```



### 963. Minimum Area Rectangle II

最多50个点，C(50, 4)也就大约230,000，感觉可以穷举。

如何判断四个点是否构成矩形：先随便找一个基准点，然后找到离它最远的点（3次计算），然后判断对角线是否相等且互相平分。

TLE了…感觉在选四个点的时候还要优化一下。

```c++
const double MAX_NUM = 999999999;

class Solution {
public:
    double dist(vector<int> p1, vector<int> p2) {
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
    }
    
    bool mid(vector<int> p1, vector<int> p2, vector<int> p3, vector<int> p4)
    {
        if(p1[0] + p2[0] == p3[0] + p4[0] && p1[1] + p2[1] == p3[1] + p4[1]) return true;
        return false;
    }
    
    double cal(vector<int> p1, vector<int> p2, vector<int> p3, vector<int> p4) {    
        double len2 = dist(p1, p2), len3 = dist(p1, p3), len4 = dist(p1, p4);
                
        if (len3 == max(max(len2, len3), len4)) {
            vector<int> t = p2;
            p2 = p3;
            p3 = t;
        } 
        else if (len4 == max(max(len2, len3), len4)) {
            vector<int> t = p2;
            p2 = p4;
            p4 = t;
        }
        
        if (abs(dist(p1, p2) - dist(p3, p4)) < 0.001 && mid(p1, p2, p3, p4)) 
            return sqrt(dist(p1, p3) * dist(p1, p4));
        else
            return MAX_NUM;
    }
    
    double minAreaFreeRect(vector<vector<int>>& points) {
        if (points.size() < 4) return 0;
        
        double ans = MAX_NUM;
        
        int N = points.size();
        
        for (int i1 = 0; i1 < N; i1++) 
            for (int i2 = i1 + 1; i2 < N; i2++) 
                for (int i3 = i2 + 1; i3 < N; i3++)
                    for (int i4 = i3 + 1; i4 < N; i4++) {
                        double t = cal(points[i1], points[i2], points[i3], points[i4]);
                        // cout << i1 << i2 << i3 << i4 << " " << t << endl;
                        if (abs(t - MAX_NUM) < 0.001) continue;
                        ans = min(ans, t);
                    }
        
        if (abs(ans - MAX_NUM) < 0.001) return 0;
        else return ans;
    }
};
```

然后…看讨论区，有人说…把第四层循环用set代替就好了....

复杂度O(N^3logN)...

```C++
const double MAX_NUM = 999999999;

class Solution {
public:
    double dist(vector<int> p1, vector<int> p2) {
        return (p1[0] - p2[0]) * (p1[0] - p2[0]) + (p1[1] - p2[1]) * (p1[1] - p2[1]);
    }
    
    double cal(vector<int> p1, vector<int> p2, vector<int> p3) {   
        return sqrt(dist(p1, p2) * dist(p1, p3));
    }
    
    bool isValid(vector<int> p1, vector<int> p2, vector<int> p3) {
        if (abs(dist(p1, p2) + dist(p1, p3) - dist(p2, p3)) < 0.001) return true;
        return false;
    }
    
    double minAreaFreeRect(vector<vector<int>>& points) {
        if (points.size() < 4) return 0;
        
        double ans = MAX_NUM;
        
        int N = points.size();
        
        // hash map
        set<pair<int, int>> hmap;
        
        for (int i = 0; i < N; i++)
            hmap.insert({points[i][0], points[i][1]});
        
        for (int i1 = 0; i1 < N; i1++) 
            for (int i2 = i1 + 1; i2 < N; i2++) 
                for (int i3 = i2 + 1; i3 < N; i3++) {
                    if (isValid(points[i1], points[i2], points[i3])) {
                        int x = points[i2][0] + points[i3][0] - points[i1][0];
                        int y = points[i2][1] + points[i3][1] - points[i1][1];
                        if (hmap.find({x, y}) != hmap.end()) {
                            ans = min(ans, cal(points[i1], points[i2], points[i3]));
                        }
                    }
                    if (isValid(points[i2], points[i1], points[i3])) {
                        int x = points[i1][0] + points[i3][0] - points[i2][0];
                        int y = points[i1][1] + points[i3][1] - points[i2][1];
                        if (hmap.find({x, y}) != hmap.end()) {
                            ans = min(ans, cal(points[i2], points[i1], points[i3]));
                        }
                    }
                    if (isValid(points[i3], points[i1], points[i2])) {
                        int x = points[i1][0] + points[i2][0] - points[i3][0];
                        int y = points[i1][1] + points[i2][1] - points[i3][1];
                        if (hmap.find({x, y}) != hmap.end()) {
                            ans = min(ans, cal(points[i3], points[i1], points[i2]));
                        }
                    }
                }
        
        if (abs(ans - MAX_NUM) < 0.001) return 0;
        else return ans;
    }
};
```





### 964. Least Operators to Express Number



```python

```





