---
title: LeetCode Weekly Contest 121
date: 2019-01-27 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Jan 27th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 981. Time Based Key-Value Store

C++ STL map中使用upper_bound做二分查找。

```c++
class TimeMap {
private:
    map<string, map<int, string>> store;
public:
    /** Initialize your data structure here. */
    TimeMap() {
        this -> store.clear();
    }
    
    void set(string key, string value, int timestamp) {
        this -> store[key][timestamp] = value;
    }
    
    string get(string key, int timestamp) {
        auto it = this -> store[key].upper_bound(timestamp);
        if (it != this -> store[key].begin() && --it != this -> store[key].end()) return it -> second;
        return "";
    }
};

/**
 * Your TimeMap object will be instantiated and called as such:
 * TimeMap* obj = new TimeMap();
 * obj->set(key,value,timestamp);
 * string param_2 = obj->get(key,timestamp);
 */
```



### 982. Triples with Bitwise AND Equal To Zero

如何避免三次循环...

要么在某一个循环的时候有个非O(n)的方法，要么以空间换时间？

使用map的话，在键值查找的时候是O(log n)的时间，会TLE。

使用unordered map，在查找的时候是O(1)的时间，就可以AC...

```c++
class Solution {
public:
    int countTriplets(vector<int>& a) {
        int n = a.size();
        unordered_map<int, int> m;
        
        for (int i = 0; i < n; ++i) 
            for (int j = 0; j < n; ++j) 
                m[a[i] & a[j]]++;
    
        int ret = 0;
        for (int i = 0; i < n; ++i)
            for (auto mm: m) {
                if (!(a[i] & (mm.first)))
                    ret += mm.second;
            }
        
        return ret;
    }
};
```



### 983. Minimum Cost For Tickets

一个简单的动态规划。

```c++
class Solution {
public:
    int mincostTickets(vector<int>& days, vector<int>& costs) {
        vector<int> f(366, 0);
        
        vector<bool> a(366, 0);
        for (int x : days) a[x] = true;
        
        if (a[1]) f[1] = costs[0];
        
        for (int i = 2; i <= 365; ++i) {
            if (a[i]) {
                if (i > 30) f[i] = min(f[i - 1] + costs[0], min(f[i - 7] + costs[1], f[i - 30] + costs[2]));
                else if (i > 7) f[i] = min(f[i - 1] + costs[0], min(f[i - 7] + costs[1], costs[2]));
                else f[i] = min(f[i - 1] + costs[0], min(costs[1], costs[2]));
            }
            else f[i] = f[i - 1];
        }
                
        return f[365];
    }
};
```



### 984. String Without AAA or BBB

```c++
class Solution {
public:
    string strWithout3a3b(int a, int b) {
        string sa = "a", sb = "b";
        if (b > a) {
            int t = a;
            a = b;
            b = t;
            sa = "b";
            sb = "a";
        }
        string ret = "";
        while (a && b && a > b) {
            ret += sa + sa + sb;
            a -= 2;
            b--;
        }
        while (a && b) {
            ret += sa + sb;
            a--;
            b--;
        }
        while (a--) ret += sa;
        while (b--) ret += sb;
    
        return ret;
    }
};
```





