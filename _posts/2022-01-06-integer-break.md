---
layout: post
title: "刷题记录-整数拆分"
subtitle: ""
date: 2022-01-06
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","DP","数学"]
---

> https://leetcode-cn.com/problems/integer-break/

# 题目

给定一个正整数 `n`，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:

```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

示例 2:

```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

- 说明: 你可以假设 n 不小于 2 且不大于 58。

# 思路

观察示例和自己测试几个数，会发现拆分只会用到 2 和 3 。因为 4 可以分成 2 * 2，对结果没有影响。任何大于 4 的数，设为 `n` ，都可以被拆成 `2 * (n - 2) = 2n - 4 > n ` ，而 1 是完全没必要的。同时对于 6 来说， `3 * 3 > 2 * 2 * 2` ，因此 2 只会被用到 1 或者 2 次。

也可以用 DP 递推。

# 代码

```c++
class Solution {
public:
    int integerBreak(int n) {
        if (n == 2) return 1;
        else if (n == 3) return 2;
        else {
            int num_3 = 0, num_2 = 0;
            if (n % 3 == 1) {
                num_2 = 2;
                num_3 = (n - 4) / 3;
            } else if (n % 3 == 2) {
                num_2 = 1;
                num_3 = (n - 2) / 3;
            } else {
                num_3 = n / 3;
            }
            return pow(2, num_2) * pow(3, num_3);
        }
    }
};
```

