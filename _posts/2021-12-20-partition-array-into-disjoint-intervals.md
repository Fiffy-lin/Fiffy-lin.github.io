---
layout: post
title: "刷题记录-分割数组"
subtitle: ""
date: 2021-12-20
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","贪心"]
---

> https://leetcode-cn.com/problems/partition-array-into-disjoint-intervals/

# 题目

给定一个数组 `A`，将其划分为两个连续子数组 `left` 和 `right`， 使得：

`left` 中的每个元素都小于或等于 `right` 中的每个元素。
`left` 和 `right` 都是非空的。
`left` 的长度要尽可能小。
在完成这样的分组后返回 `left` 的长度。可以保证存在这样的划分方法。

 

示例 1：

```
输入：[5,0,3,8,6]
输出：3
解释：left = [5,0,3]，right = [8,6]
```

示例 2：

```
输入：[1,1,1,0,6,12]
输出：4
解释：left = [1,1,1,0]，right = [6,12]
```


提示：

- `2 <= A.length <= 30000`
- `0 <= A[i] <= 10^6`
- 可以保证至少有一种方法能够按题目所描述的那样对 `A` 进行划分。

# 思路

对每一个分割点，维护左侧最大值和右侧最小值。由于题目要求 `left` 长度最小，从左侧开始累积即可。

# 代码

```c++
class Solution {
public:
    int partitionDisjoint(vector<int> &nums) {
        int n = nums.size();
        vector<int> leftmax(n, 0), rightmin(n, 0);
        leftmax[1] = nums[0];
        for (int i = 2; i < n; ++i) {
            leftmax[i] = max(nums[i - 1], leftmax[i - 1]);
        }
        rightmin[n - 1] = nums[n - 1];
        for (int i = n - 2; i >= 0; --i) {
            rightmin[i] = min(nums[i], rightmin[i + 1]);
        }
        for (int i = 1; i < n; ++i) {
            if (leftmax[i] <= rightmin[i]) return i;
        }
        return 0;
    }
};
```

