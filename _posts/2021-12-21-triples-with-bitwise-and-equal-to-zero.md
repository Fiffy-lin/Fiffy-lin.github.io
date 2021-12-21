---
layout: post
title: "刷题记录-按位与为零的三元组"
subtitle: ""
date: 2021-12-21
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","哈希表","位运算"]
---

> https://leetcode-cn.com/problems/triples-with-bitwise-and-equal-to-zero/

# 题目

给定一个整数数组 A，找出索引为 `(i, j, k)` 的三元组，使得：

- `0 <= i < A.length`
- `0 <= j < A.length`
- `0 <= k < A.length`
- `A[i] & A[j] & A[k] == 0`，其中 & 表示按位与（AND）操作符。


示例：

```
输入：[2,1,3]
输出：12
解释：我们可以选出如下 i, j, k 三元组：
(i=0, j=0, k=1) : 2 & 2 & 1
(i=0, j=1, k=0) : 2 & 1 & 2
(i=0, j=1, k=1) : 2 & 1 & 1
(i=0, j=1, k=2) : 2 & 1 & 3
(i=0, j=2, k=1) : 2 & 3 & 1
(i=1, j=0, k=0) : 1 & 2 & 2
(i=1, j=0, k=1) : 1 & 2 & 1
(i=1, j=0, k=2) : 1 & 2 & 3
(i=1, j=1, k=0) : 1 & 1 & 2
(i=1, j=2, k=0) : 1 & 3 & 2
(i=2, j=0, k=1) : 3 & 2 & 1
(i=2, j=1, k=0) : 3 & 1 & 2
```


提示：

- `1 <= A.length <= 1000`
- `0 <= A[i] < 2^16`

# 思路

直接三重循环肯定是行不通的，我们像三数之和一样，预处理好两位数相与的结果。

对于某一个二进制数 `x`，在数据范围内存在最大的 `y`，使两者相与为 0，那么 `x + y` 一定等于数据范围的上限。比如上限是 `1111 1111` , `x = 1001 0011` , 则最大的 `y` 一定是 `0110 1100` 。

因此对于每个 `x`，我们首先得到 `y` 的最大值，可以发现从 `y` 里面去掉某一位 `1` 不会影响相与结果，因此持续累加答案，直到 `y` 为 0 。

# 代码

```c++
class Solution {
public:
    int countTriplets(vector<int> &nums) {
        int maxNum = 1 << 16;
        int cnt[maxNum];
        memset(cnt, 0, sizeof(cnt));
        int n = nums.size();

        // 对于两个数字与结果计数
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                ++cnt[nums[i] & nums[j]];
            }
        }

        // 遍历每个数字去找满足条件的数量
        int res = 0;
        for (int i = 0; i < n; ++i) {
            int full = maxNum - 1 - nums[i];
            // 遍历各种bit情况去计算: 移除最后一个数字
            // 这里得注意： 因为数字每次会改变，所以得基于原始值去与，否则会漏掉很多数字
            for (int j = full; j > 0; j = (j - 1) & full) {
                res += cnt[j];
            }

            // 不要忘记加上0的情况，任何数字与0与都是0
            res += cnt[0];
        }
        return res;
    }
};
```

