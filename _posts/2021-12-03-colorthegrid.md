---
layout: post
title: "刷题记录-用三种不同颜色为网格涂色"
subtitle: ""
date: 2021-12-03
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","DP","哈希表","状态压缩"]
---

> https://leetcode-cn.com/problems/painting-a-grid-with-three-different-colors/

# 题目

给你两个整数 `m` 和 `n` 。构造一个 `m x n` 的网格，其中每个单元格最开始是白色。请你用 红、绿、蓝 三种颜色为每个单元格涂色。所有单元格都需要被涂色。

涂色方案需要满足：不存在相邻两个单元格颜色相同的情况 。返回网格涂色的方法数。因为答案可能非常大， 返回 对 `10^9 + 7` 取余 的结果。

示例 1：

![](https://assets.leetcode.com/uploads/2021/06/22/colorthegrid.png)

```
输入：m = 1, n = 1
输出：3
解释：如上图所示，存在三种可能的涂色方案。
```

示例 2：

![](https://assets.leetcode.com/uploads/2021/06/22/copy-of-colorthegrid.png)

```
输入：m = 1, n = 2
输出：6
解释：如上图所示，存在六种可能的涂色方案。
```


示例 3：

```
输入：m = 5, n = 5
输出：580986
```


提示：

- `1 <= m <= 5`
- `1 <= n <= 1000`

# 思路

m 很小，n 很大。同时我们注意到本行的数量是可以由上一行推出来的。所以我们枚举每行的所有状态来维护 DP 数组。

一个格子只能有三种颜色，可以用三进制动态压缩。一行内所有状态的数量就是 `pow(3, m)` 。我们先处理好行内所有合法（即左右没有相邻颜色）的状态，再处理好行间合法（即上下不相邻）的状态，就可以进行状态转移了。

最后，因为状态转移时本行仅和上一行有关，因此可以用两个数组枚举所有行，把空间复杂度压缩到一维。

# 代码

```c++
class Solution {
private:
    static constexpr int mod = 1000000007;

public:
    int colorTheGrid(int m, int n) {
        int decimal_cnt = pow(3, m);
        unordered_map<int, vector<int>> valid_decimal_to_digit;
        for (int decimal = 0; decimal < decimal_cnt; ++decimal) {
            vector<int> digits;;
            for (int i = 0, curr = decimal; i < m; ++i) {
                digits.push_back(curr % 3);
                curr /= 3;
            }
            bool valid = true;
            for (int i = 1; i < digits.size() && valid; ++i) {
                if (digits[i] == digits[i - 1]) valid = false;
            }
            if (valid) valid_decimal_to_digit[decimal] = move(digits);
        }
        unordered_map<int, vector<int>> decimal_to_adjacent_decimal;
        for (auto &[decimal1, digit1]: valid_decimal_to_digit) {
            for (auto &[decimal2, digit2]: valid_decimal_to_digit) {
                bool valid = 1;
                for (int i = 0; i < m && valid; ++i) {
                    if (digit1[i] == digit2[i]) valid = false;
                }
                if (valid) decimal_to_adjacent_decimal[decimal1].push_back(decimal2);
            }
        }

        vector<int> prev(decimal_cnt, 0);
        for (auto &[decimal, _]: valid_decimal_to_digit) prev[decimal] = 1;
        for (int row = 1; row < n; ++row) {
            vector<int> curr(decimal_cnt);
            for (const auto&[mask2, _]: valid_decimal_to_digit) {
                for (int mask1: decimal_to_adjacent_decimal[mask2]) {
                    curr[mask2] += prev[mask1];
                    if (curr[mask2] >= mod) {
                        curr[mask2] -= mod;
                    }
                }
            }
            prev = move(curr);
        }
        int res = 0;
        for (auto cnt: prev) {
            res += cnt;
            if (res > mod) res -= mod;
        }
        return res;
    }
};
```

