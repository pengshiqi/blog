---
title: 'LeetCode 72: Edit Distance'
date: 2019-06-25 11:29:01
tags: [LeetCode, Dynamic Programming]
categories: [LeetCode, Dynamic Programming]
---

#### **题目大意：**

[Link](<https://leetcode.com/problems/edit-distance/>)

给定两个字符串word1和word2，将word1转换为word2最少需要多少次操作？

<!-- more -->

操作分为三种：

* 插入一个字母 (insert)
* 删除一个字母 (delete)
* 替换一个字母 (replace)

Example 1:  

```
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
```

Example 2:

```
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
```



#### Solution:

首先，我们定义状态：`d[i][j]` 表示将 `word1[0...i)` 转换为 `word2[0...j)` 所需要的最少次数。

对于基础情形，将一个字符串转换为空字符串需要的操作数：`d[i][0] = i` ， `d[0][j] = j`。

对于一般情形，将 `word1[0...i)` 转换为 `word2[0...j)` ，我们将这个问题分解为子问题：

假设我们已经知道如何将 `word1[0..i-1)` 转换为 `word2[0...j-1]`(即`dp[i-1][j-1]`)，

如果`word1[i-1]==word2[j-1]`，那么不需要进行额外的操作，所以`dp[i][j]=dp[i-1][j-1]`。

如果`word1[i-1]!=word2[j-1]`，此时有三种情况：

1. Replace: 将`word1[i-1]`替换为`word2[j-1]`。 (`dp[i][j] = dp[i-1][j-1] + 1`)
2. Delete: 如果`word1[0...i-1) == word2[0...j)`，那么删除`word1[i-1]`。(`dp[i][j] = dp[i-1][j] + 1`)
3. Insert: 如果`word1[0...i)+word2[j-1] == word2[0...j)`，那么在`word1[0...i)`后面插入`word2[j - 1]`。(`dp[i][j] = dp[i][j-1] + 1`)

所以最终的`dp[i][j]`为以上三种情况的最小值。



C++代码：

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; ++i) dp[i][0] = i;
        for (int i = 1; i <= n; ++i) dp[0][i] = i;
        
        for (int i = 1; i <= m; ++i) {
            for (int j = 1; j <= n; ++j) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                }
                else {
                    dp[i][j] = min(dp[i - 1][j - 1], min(dp[i - 1][j], dp[i][j - 1])) + 1;
                }
            }
        }
        
        return dp[m][n];
    }
};
```



