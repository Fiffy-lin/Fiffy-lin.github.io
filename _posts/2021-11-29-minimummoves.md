---
layout: post
title: "刷题记录-删除回文子数组"
subtitle: ""
date: 2021-11-29
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DP","区间DP"]
---

# 题目

给你一个整数数组 arr，每一次操作你都可以选择并删除它的一个 回文 子数组 `arr[i], arr[i+1], ..., arr[j]（ i <= j）`。

注意，每当你删除掉一个子数组，右侧元素都会自行向前移动填补空位。

请你计算并返回从数组中删除所有数字所需的最少操作次数。

 

示例 1：

```
输入：arr = [1,2]
输出：2
```

示例 2：

```
输入：arr = [1,3,4,1,5]
输出：3
解释：先删除 [4]，然后删除 [1,3,1]，最后再删除 [5]。
```


提示：

- `1 <= arr.length <= 100`
- `1 <= arr[i] <= 20`

# 思路

先套区间 DP 的模版，`dp[i][j]` 就表示删除下标 i 到下标 j 范围内子数组所用的最少操作次数。枚举区间中点来更新这个值。

考虑回文情况下的删除，回文情况下最外层可以和里面一起删，所以再更新一下回文情况下的删除次数。

```c++
class Solution {
public:
    int minimumMoves(vector<int> &arr) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(n, INT_MAX));
        for (int i = 0; i < n; ++i) dp[i][i] = 1;
        for (int i = 0; i < n - 1; ++i) if (arr[i] == arr[i + 1]) dp[i][i + 1] = 1; else dp[i][i + 1] = 2;
        for (int length = 3; length <= n; ++length) {
            for (int s = 0, e = s + length - 1; e < n; ++s, ++e) {
                for (int k = s; k < e; ++k)
                    dp[s][e] = min(dp[s][k] + dp[k + 1][e], dp[s][e]);
                if (arr[s] == arr[e]) dp[s][e] = min(dp[s][e], dp[s + 1][e - 1]);
            }
        }
        return dp[0][n - 1];
    }
};
```

