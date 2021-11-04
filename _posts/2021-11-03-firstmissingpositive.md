---
layout: post
title: "刷题记录-缺失的第一个正数"
subtitle: ""
date: 2021-11-03
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。


示例 1：

```
输入：nums = [1,2,0]
输出：3
```

示例 2：

```
输入：nums = [3,4,-1,1]
输出：2
```

示例 3：

```
输入：nums = [7,8,9,11,12]
输出：1
```


提示：

- `1 <= nums.length <= 5 * 10^5`
- `-2^31 <= nums[i] <= 2^31 - 1`

# 思路

`n` 表示数组长度，如果数组元素是 1-n 的某个全排列，那么应当返回 `n+1` ，如果不是，那么返回 1-n 中的某个数字。因为只能用常数级别额外空间，考虑原地哈希。

对于数组中一个值为 x 的数，我们需要标记这个数出现了，考虑把位置 x-1 的数设为负数。为了避免数组中本身存在的负数的影响，我们把它们变成一个超过 n+1 的正数即可。同时要注意不能重复取负号。

# 代码

```c++
class Solution {
public:
    int firstMissingPositive(vector<int> &nums) {
        for (int i = 0; i < nums.size(); ++i) if (nums[i] <= 0) nums[i] = nums.size() + 1;
        for (int i = 0; i < nums.size(); ++i) {
            int val = abs(nums[i]);
            if (val <= nums.size()) nums[val - 1] = -1 * abs(nums[val - 1]);
        }
        for (int i = 0; i < nums.size(); ++i) if (nums[i] > 0) return i + 1;
        return nums.size() + 1;
    }
};
```

