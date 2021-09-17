---
layout: post
title: "刷题记录 回旋镖"
subtitle: "枚举-哈希"
date: 2021-09-13
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","哈希表"]
---

# 题目描述

给定平面上 n 对 互不相同 的点 points ，其中 points[i] = [xi, yi] 。回旋镖 是由点 (i, j, k) 表示的元组 ，其中 i 和 j 之间的距离和 i 和 k 之间的距离相等（需要考虑元组的顺序）。

返回平面上所有回旋镖的数量。

示例 1：

```
输入：points = [[0,0],[1,0],[2,0]]
输出：2
解释：两个回旋镖为 [[1,0],[0,0],[2,0]] 和 [[1,0],[2,0],[0,0]]
```


示例 2：

```
输入：points = [[1,1],[2,2],[3,3]]
输出：2
```

示例 3：

```
输入：points = [[1,1]]
输出：0
```


提示：

- `n == points.length`
- `1 <= n <= 500`
- `points[i].length == 2`
- `-104 <= xi, yi <= 104`
- 所有点都 互不相同

# 思路

一开始是打算暴力遍历所有三元组，算出每对点间的距离。为了避免重复计算，使用哈希表优化，超时。

想了一下，使用三重循环遍历三元组可以优化，因为题目只要求返回三元组的数量。所以算距离只需要二重循环遍历点对，然后对相同距离的点对计算数量就可以了。
$$
triplet\_count = count*(count-1)
$$
同时，存距离的时候也不需要开根号，直接存平方就行了。这样可以把`double` 全部换成`int`。

# Code

```c++
class Solution {
public:
    int calculateDistance(vector<int> &p1, vector<int> &p2) {
        return pow(p1[0] - p2[0], 2) + pow(p1[1] - p2[1], 2);
    }

    int numberOfBoomerangs(vector<vector<int>> &points) {
        int ans = 0;
        map<int, int> disCount;
        for (auto &p: points) {
            disCount.clear();
            for (auto &q: points) {
                disCount[calculateDistance(p, q)]++;
            }
            for (auto &pair: disCount) {
                ans += pair.second * (pair.second - 1);
            }
        }
        return ans;
    }
};
```

