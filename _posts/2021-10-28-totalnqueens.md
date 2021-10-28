---
layout: post
title: "刷题记录-N皇后 II"
subtitle: ""
date: 2021-10-28
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","回溯"]
---

# 题目

`n` 皇后问题 研究的是如何将 `n` 个皇后放置在 `n×n` 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 `n` ，返回 `n` 皇后问题 不同的解决方案的数量。

示例 1：

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

```
输入：n = 4
输出：2
解释：如上图所示，4 皇后问题存在两个不同的解法。
```


示例 2：

```
输入：n = 1
输出：1
```


提示：

- `1 <= n <= 9`
- 皇后彼此不能相互攻击，也就是说：任何两个皇后都不能处于同一条横行、纵行或斜线上。

# 思路

回溯，枚举当前行所有列，检查是否可以放入。

注意对二维数组遍历时要想清楚下标和坐标轴的对应关系。

# 代码

```c++
class Solution {
public:
    char board[9][9] = {0};
    int _n, res = 0;

    bool check(int a, int b) {
        // check top-left
        for (int x = a - 1, y = b - 1; x >= 0 && y >= 0; x--, y--) if (board[x][y]) return false;
        // check top
        for (int x = a - 1, y = b; x >= 0; x--) if (board[x][y]) return false;
        // check top-right
        for (int x = a - 1, y = b + 1; x >= 0 && y < _n; x--, y++) if (board[x][y]) return false;
        return true;
    }

    void backtrace(int num_queens) {
        if (num_queens == _n) {
            res++;
            return;
        }
        int x = num_queens;
        for (int y = 0; y < _n; ++y) {
            if (check(x, y)) {
                board[x][y] = 1;
                backtrace(num_queens + 1);
                board[x][y] = 0;
            }
        }
    }

    int totalNQueens(int n) {
        _n = n;
        backtrace(0);
        return res;
    }
};
```

