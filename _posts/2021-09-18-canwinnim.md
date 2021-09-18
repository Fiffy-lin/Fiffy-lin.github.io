---
layout: post
title: "刷题记录-Nim 游戏"
subtitle: ""
date: 2021-09-18
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DP","数学"]
---

# 题目描述

你和你的朋友，两个人一起玩 Nim 游戏：

桌子上有一堆石头。
你们轮流进行自己的回合，你作为先手。
每一回合，轮到的人拿掉 1 - 3 块石头。
拿掉最后一块石头的人就是获胜者。
假设你们每一步都是最优解。请编写一个函数，来判断你是否可以在给定石头数量为 n 的情况下赢得游戏。如果可以赢，返回 `true`；否则，返回` false `。

示例 1：

```
输入：n = 4
输出：false 
解释：如果堆中有 4 块石头，那么你永远不会赢得比赛；
     因为无论你拿走 1 块、2 块 还是 3 块石头，最后一块石头总是会被你的朋友拿走。
```

示例 2：

```
输入：n = 1
输出：true
```

示例 3：

```
输入：n = 2
输出：true
```


提示：

- `1 <= n <= 2^31 - 1`

# 思路

一开始想到的方法是 DP。状态转移方程是 `dp[i] = !(dp[i - 1] && dp[i - 2] && dp[i - 3]);` 只要存在拿掉 1/2/3 个石子时对面赢不了的情况，自己就能赢。但是这也超时了。

手算了几个，`i`在取 1/2/3 时结果都是 `true`，`i=4`时`dp[i]=false`，因此`dp[5] = dp[6] = dp[7] = true`，以此类推，就找到了数学规律，不能被 4 整除，就可以赢。

# 代码

## DP

```c++
class Solution {
public:
    bool canWinNim(int n) {
        if (n < 4) return true;
        vector<bool> dp(n + 1, 0);
        dp[1] = dp[2] = dp[3] = true;
        for (int i = 4; i <= n; ++i) {
            dp[i] = !(dp[i - 1] && dp[i - 2] && dp[i - 3]);
        }
        return dp[n];
    }
};
```

## 规律

```c++
class Solution {
public:
    bool canWinNim(int n) {
        return n & 3;
    }
};
```

