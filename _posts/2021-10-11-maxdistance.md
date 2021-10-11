---
layout: post
title: "刷题记录-数组列表中的最大距离"
subtitle: ""
date: 2021-10-11
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给定 `m` 个数组，每个数组都已经按照升序排好序了。现在你需要从两个不同的数组中选择两个整数（每个数组选一个）并且计算它们的距离。两个整数 `a` 和 `b` 之间的距离定义为它们差的绝对值 `|a-b|` 。你的任务就是去找到最大距离

示例 1：

```
输入： 
[[1,2,3],
 [4,5],
 [1,2,3]]
输出： 4
解释：
一种得到答案 4 的方法是从第一个数组或者第三个数组中选择 1，同时从第二个数组中选择 5 。
```


注意：

- 每个给定数组至少会有 1 个数字。列表中至少有两个非空数组。
- 所有 `m` 个数组中的数字总数目在范围 `[2, 10000]` 内。
- `m` 个数组中所有整数的范围在`[-10000, 10000]` 内。

# 思路

暴力搜会超时。

因为最大和最小不能在同一个数组里，所以先寻找差值，再更新最大和最小值。

差值有三个备选项：前一个差值，当前最大 - 过去最小，过去最大 - 当前最小。这样循环一遍可以达到之前暴力搜索的效果。

# 代码

```c++
class Solution {
public:
    int maxDistance(vector<vector<int>> &arrays) {
        int min_value = arrays[0].front();
        int max_value = arrays[0].back();
        int res = 0;
        for (int i = 1; i < arrays.size(); ++i) {
            res = max(res, max(arrays[i].back() - min_value, max_value - arrays[i].front()));
            min_value = min(min_value, arrays[i].front());
            max_value = max(max_value, arrays[i].back());
        }
        return res;
    }
};
```

