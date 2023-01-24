---
title: LeetCode Weekly Contest 128
date: 2019-03-17 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Mar 17th, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->



### 1012. Complement of Base 10 Integer

```c++
class Solution {
public:
    vector<int> getBinary(int n) {
        vector<int> r;
        if (n == 0) {
            r.push_back(0);
            return r;
        }
        
        while (n) {
            if (n % 2) r.push_back(1);
            else r.push_back(0);
            n /= 2;
        }
        return r;
    }
    
    vector<int> getComplement(vector<int>& v) {
        for (auto &x : v) {
            x = 1 - x;
        }
        return v;
    }
    
    int getBase10(vector<int>& v) {
        int r = 0, base = 1;
        for (int i = 0; i < v.size(); ++i) {
            r += v[i] * base;
            base *= 2;
        }
        return r;
    }
    
    int bitwiseComplement(int N) {
        vector<int> bin, com;
        bin = getBinary(N);
        com = getComplement(bin);
        int ret;
        ret = getBase10(com);
        return ret;
    }
};
```



### 1013. Pairs of Songs With Total Durations Divisible by 60

```c++
class Solution {
public:
    int numPairsDivisibleBy60(vector<int>& time) {
        vector<int> c(60, 0);
        for (auto &x: time) c[x % 60]++;
        
        int ret = 0;
        ret += (c[0] * (c[0] - 1)) / 2;
        ret += (c[30] * (c[30] - 1)) / 2;
        
        for (int i = 1; i <= 29; ++i) ret += c[i] * c[60 - i];
        
        return ret;
    }
};
```



### 1014. Capacity To Ship Packages Within D Days 

这题有点意思，将一段序列切分成D个子序列，求最大子序列和的最小值是多少。

通过率这么高...应该也不是很难...

好菜啊...好不容易攒的一点分要掉下去了...

看了别人代码…就是个二分法...

```c++

```



### 1015. Numbers With 1 Repeated Digit

```c++

```





