---
layout: post
title: "刷题记录-分割数组的最多方案数"
subtitle: ""
date: 2021-11-23
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","前缀和","哈希表"]
---

#  题目

给你一个下标从 0 开始且长度为 `n` 的整数数组 `nums` 。分割 数组 `nums` 的方案数定义为符合以下两个条件的 `pivot` 数目：

- `1 <= pivot < n`
- `nums[0] + nums[1] + ... + nums[pivot - 1] == nums[pivot] + nums[pivot + 1] + ... + nums[n - 1]`

同时给你一个整数 k 。你可以将 `nums` 中 一个 元素变为 `k` 或 不改变 数组。

请你返回在 至多 改变一个元素的前提下，最多 有多少种方法 分割 `nums` 使得上述两个条件都满足。

 

示例 1：

```
输入：nums = [2,-1,2], k = 3
输出：1
解释：一个最优的方案是将 nums[0] 改为 k 。数组变为 [3,-1,2] 。
有一种方法分割数组：

pivot = 2 ，我们有分割 [3,-1 | 2]：3 + -1 == 2 。
```

示例 2：

```
输入：nums = [0,0,0], k = 1
输出：2
解释：一个最优的方案是不改动数组。
有两种方法分割数组：

pivot = 1 ，我们有分割 [0 | 0,0]：0 == 0 + 0 。

pivot = 2 ，我们有分割 [0,0 | 0]: 0 + 0 == 0 。
```

示例 3：

```
输入：nums = [22,4,-25,-20,-15,15,-16,7,19,-10,0,-13,-14], k = -33
输出：4
解释：一个最优的方案是将 nums[2] 改为 k 。数组变为 [22,4,-33,-20,-15,15,-16,7,19,-10,0,-13,-14] 。
有四种方法分割数组。
```


提示：

- `n == nums.length`
- `2 <= n <= 10^5`
- `-10^5 <= k, nums[i] <= 10^5`

# 思路

首先处理好前缀和，用 `left` `right` 两个哈希表存储**枚举点左侧点和枚举点右侧点**的**右半部分减左半部分差值**。在枚举开始前，所有的点都位于枚举点右侧。

枚举当前点时，当前点的值由 `num[i]` 变成了 `k` ，因此：

- 对于左侧点，左边部分的和不变，右边部分的和多了 `k - num[i]` ，因此差值多了 `k - num[i]` , 由要求 右 +  `k - num[i]`  - 左 = 0 得知，右减左等于 `num[i] - k`
- 对于右侧点，左边和多了 `k - num[i]` 和前面正好反过来。

因此累加左右侧差值对应的哈希表值就得到了当前位改了之后的结果。

处理完这一位之后，要把当前位的左右差值从 `right` 中放到 `left` ，因为对于待枚举的下一位，本位已经位于其左侧了。

# 代码

```c++
class Solution {
public:
    int waysToPartition(vector<int> &nums, int k) {
        int n = nums.size();
        vector<long long> prefix(n);
        prefix[0] = nums[0];
        long long ans = 0;
        for (int i = 1; i < n; ++i) {
            prefix[i] = prefix[i - 1] + nums[i];
        }
        unordered_map<long long, long long> left, right; // 保存以i为区分的左、右差值的哈希表
        for (int i = 0; i < n - 1; ++i) {
            long long cnt = prefix[n - 1] - 2 * prefix[i];
            right[cnt]++;
            if (cnt == 0) {
                ++ans;
            }
        }
        for (int i = 0; i < n; ++i) {
            long long cnt = prefix[n - 1] - 2 * prefix[i];
            ans = max(ans, left[nums[i] - k] + right[k - nums[i]]);
            left[cnt]++; // 遍历时更新2个哈希表
            right[cnt]--;
        }
        return ans;
    }
};
```

