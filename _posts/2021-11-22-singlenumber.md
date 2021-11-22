---
layout: post
title: "刷题记录-只出现一次的数字"
subtitle: ""
date: 2021-11-22
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","位运算"]
---

# 题目

给定一个非空整数数组，除了某个元素只出现一次以外，其余每个元素均出现两次。找出那个只出现了一次的元素。

说明：

你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？

示例 1:

```
输入: [2,2,1]
输出: 1
```

示例 2:

```
输入: [4,1,2,1,2]
输出: 4
```

# 思路

异或秒了。

# 代码

```c++
class Solution {
public:
    int singleNumber(vector<int> &nums) {
        int val = 0;
        for (auto &x: nums) val ^= x;
        return val;
    }
};
```

