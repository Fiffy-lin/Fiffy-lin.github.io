---
layout: post
title: "刷题记录-按权重随机选择"
subtitle: ""
date: 2021-10-26
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二分"]
---

# 题目

给定一个正整数数组 `w` ，其中 `w[i]` 代表下标 `i` 的权重（下标从 0 开始），请写一个函数 `pickIndex` ，它可以随机地获取下标 `i`，选取下标 `i` 的概率与 `w[i]` 成正比。

例如，对于 `w = [1, 3]`，挑选下标 0 的概率为 1 / (1 + 3) = 0.25 （即，25%），而选取下标 1 的概率为 3 / (1 + 3) = 0.75（即，75%）。

也就是说，选取下标 `i` 的概率为 `w[i] / sum(w)` 。

示例 1：

输入：

```
["Solution","pickIndex"]
[[[1]],[]]
输出：
[null,0]
解释：
Solution solution = new Solution([1]);
solution.pickIndex(); // 返回 0，因为数组中只有一个元素，所以唯一的选择是返回下标 0。
```

示例 2：

```
输入：
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
输出：
[null,1,1,1,1,0]
解释：
Solution solution = new Solution([1, 3]);
solution.pickIndex(); // 返回 1，返回下标 1，返回该下标概率为 3/4 。
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 1
solution.pickIndex(); // 返回 0，返回下标 0，返回该下标概率为 1/4 。

由于这是一个随机问题，允许多个答案，因此下列输出都可以被认为是正确的:
[null,1,1,1,1,0]
[null,1,1,1,1,1]
[null,1,1,1,0,0]
[null,1,1,1,0,1]
[null,1,0,1,0,0]
......
诸若此类。
```


提示：

- `1 <= w.length <= 10000`
- `1 <= w[i] <= 10^5`
- `pickIndex` 将被调用不超过 10000 次

# 思路

我们维护一个前缀和数组，生成 (0, sum) 范围内的一个随机数，看这个数落到前缀和数组里的哪个区间。下标 0 的前缀和是这个数，因此如果生成的随机数小于这个数，应该返回 0 。问题就转换成了二分查找第一个大于目标值的下标的过程。

# 代码

```c++
class Solution {
    vector<int> w;
public:
    Solution(vector<int> &W) {
        partial_sum(W.begin(), W.end(), back_inserter(w));
        //等同于 w.push_back(W[0]); for(int i = 1; i < W.size(); ++i) w.push_back(w.back() + W[i]);
    }

    int pickIndex() {
        int weight = rand() % w.back();
        int lo = 0, hi = w.size() - 1;
        while (lo != hi) {
            int mid = (lo + hi) / 2;
            if (weight >= w[mid]) lo = mid + 1;
            else hi = mid;
        }
        return lo;
    }
};
```

