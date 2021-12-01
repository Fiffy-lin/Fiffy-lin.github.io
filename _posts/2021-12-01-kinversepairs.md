---
layout: post
title: "刷题记录-K个逆序对数组"
subtitle: ""
date: 2021-12-01
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","DP"]
---

> https://leetcode-cn.com/problems/k-inverse-pairs-array/

# 题目

给出两个整数 `n` 和 `k`，找出所有包含从 `1` 到 `n` 的数字，且恰好拥有 `k` 个逆序对的不同的数组的个数。

逆序对的定义如下：对于数组的第i个和第 j个元素，如果满 `i < j`且 `a[i] > a[j]`，则其为一个逆序对；否则不是。

由于答案可能很大，只需要返回 答案 mod `10^9 + 7` 的值。

示例 1:

```
输入: n = 3, k = 0
输出: 1
解释: 
只有数组 [1,2,3] 包含了从1到3的整数并且正好拥有 0 个逆序对。
```

示例 2:

```
输入: n = 3, k = 1
输出: 2
解释: 
数组 [1,3,2] 和 [2,1,3] 都有 1 个逆序对。
```

说明:

-  n 的范围是 [1, 1000] 并且 k 的范围是 [0, 1000]。

# 思路

DP！令 `dp[i][j]` 表示长度为 i，逆序对为 j 的组合数量，假设目前数组长度为 `n` , 最后一个数字为 x，则逆序对可以由两部分组成：

1. 数组中 `x + 1` 到 `n - 1` 这部分数和 `x` 构成的逆序，一共有 `n-x` 个
2. 数组中 1 到 `x - 1` 和 `x + 1` 到 `n - 1` 这两部分构成的逆序。把 `x + 1` 到 `n - 1` 这部分减个 1 对这部分没有任何影响，等价于 1 到 `n - 2` 这部分构成的逆序。

也就是说，我们的状态转移方程已经有了：`for x in (1, n) dp[i][j] += dp[i - 1][j - (n - x)] ` 。再把 `n - x` 和 `x` 的范围 转换一下，就得到 `for x in (0, i - 1) dp[i][j] += dp[i - 1][j - x]` 。

这样时间复杂度还是很高，因为每次状态转移都要计算一个循环。我们把 `dp[i][j]` 和 `dp[i-1][j]` 展开，可以发现 `dp[i][j] = ((j - 1 >= 0 ? dp[i][j - 1] : 0) + dp[i - 1][j] - (j >= i ? dp[i - 1][j - i] : 0))`

还可以进一步把第一维优化成常数。

另外，因为转移过程存在减法，要注意中间结果为负数的时候加上 `MOD` 。

# 代码

```c++
class Solution {
public:
    const int MOD = 1000000000 + 7;

    int kInversePairs(int n, int k) {
        vector<vector<int>> dp(n + 1, vector<int>(k + 1, 0));
        for (int i = 0; i <= n; ++i) dp[i][0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j <= k; ++j) {
                dp[i][j] = ((j - 1 >= 0 ? dp[i][j - 1] : 0) + dp[i - 1][j] - (j >= i ? dp[i - 1][j - i] : 0)) % MOD;
                if (dp[i][j] < 0) dp[i][j] += MOD;
            }
        }
        return dp[n][k];
    }
};
```

