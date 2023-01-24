---
title: LeetCode Weekly Contest 119
date: 2019-01-13 12:04:21
tags: LeetCode
categories: 
- 学习
---



Time: Jan 13rd, 2019 @ 10:30 AM - 12:00 AM  (GMT+8)

<!-- more -->

### 973. K Closest Points to Origin

把这些点按距离排序就好了。

```c++
class Solution {
public:
    static bool compare(const pair<int, int>& a, const pair<int, int>& b) {
        return a.second < b.second;
    }
    
    vector<vector<int>> kClosest(vector<vector<int>>& points, int K) {
        vector<pair<int, int>> dist;
        for (int i = 0; i < points.size(); i++) {
            dist.push_back(make_pair(i, points[i][0] * points[i][0] + points[i][1] * points[i][1]));
        }
        
        sort(dist.begin(), dist.end(), compare);
        
        vector<vector<int>> ret;
        for (int i = 0; i < K; i++) {
            ret.push_back(points[dist[i].first]);
        }
        return ret;
    }
};
```



### 974. Subarray Sums Divisible by K

这个题目的意思是求出连续子区间和能被K整除的个数。

先求一遍前i个数的和，sum数组，那么，连续子区间[l, r]的和就为 sum[r] - sum[l - 1]。

 (sum[r] - sum[l - 1]) % K = 0等价于 sum[r] % K = sum[l - 1] % K。

统计出满足上式的pair数即可。

```c++
int sum[30001];
int cnt[10001];

class Solution {
public:
    int subarraysDivByK(vector<int>& A, int K) {
        memset(sum, 0, sizeof(sum));
        sum[0] = ((A[0] % K) + K) % K;
        for (int i = 1; i < A.size(); ++i) {
            sum[i] = ((sum[i - 1] + A[i]) % K + K) % K;
        }
                        
        memset(cnt, 0, sizeof(cnt));
        cnt[0] = 1;
        int ret = 0;
        for (int i = 0; i < A.size(); ++i) {
            ret += cnt[sum[i]];
            cnt[sum[i]]++;
        }
        
        return ret;
    }
};
```



### 975. Odd Even Jump

这题想到了一个算法，但是没有时间去实现了。

其实指定了起点之后，跳跃的方式是固定的，所以问题就是如何求出当前点的odd jump位置和even jump位置。

维护一个列表B，将A倒序往B里插入，倒序可以保证插入一个数时，B中已有的数都是这个数后面的数，插入的时候保证B的顺序，这样插入后，比它小一个的数就是even jump位置，比它大一个的数就是odd jump的位置。

二分插入的复杂度是O(logn)，总的复杂度是O(nlogn)。



后来看了别人的题解，果然和我想的思路一样，使用stl的lower_bound和upper_bound可以很方便地实现。

```c++
class Solution {
public:
    int oddEvenJumps(vector<int>& a) {
        int n = a.size();
        vector<int> odd_next(n, -1), even_next(n, -1);
        map<int, int> m;
        
        for (int i = n - 1; i >= 0; --i) {
            auto it = m.lower_bound(a[i]);
            if (it != m.end()) {
                odd_next[i] = it -> second;
            }
            it = m.upper_bound(a[i]);
            // 小于等于k最大值的位置，可以直接用upper_bound() - 1来确定。
            if (it != m.begin() && --it != m.end()) {
                even_next[i] = it -> second;
            }
            m[a[i]] = i;
        }
        
        vector<bool> odd(n, false), even(n, false);
        odd[n - 1] = true;
        even[n - 1] = true;
        int ret = 1;
        for (int i = n - 2; i >= 0; --i) {
            if (odd_next[i] != -1) {
                odd[i] = even[odd_next[i]];
            }
            if (even_next[i] != -1) {
                even[i] = odd[even_next[i]];
            }
            if (odd[i]) {
                ret++;
            }
        }
        
        return ret;
    }
};
```



### 976. Largest Perimeter Triangle

求最大的三角形周长。

先将A降序排列，从左往右每三个check一次能否构成三角形就可以了，可以证明这个贪心是正确的。

```python
class Solution {
public:
    int largestPerimeter(vector<int>& a) {
        sort(a.rbegin(), a.rend());
        int ret = 0;
        for (int i = 0; i < a.size() - 2; i++) {
            if (a[i + 1] + a[i + 2] > a[i]) {
                ret = a[i] + a[i + 1] + a[i + 2];
                break;
            }
        }
        return ret;
    }
};
```





