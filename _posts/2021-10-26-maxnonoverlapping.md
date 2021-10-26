---
layout: post
title: "刷题记录-和为目标值且不重叠的非空子数组的最大数目"
subtitle: ""
date: 2021-10-26
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","前缀和","贪心"]
---

# 题目

给你一个数组 `nums` 和一个整数 `target` 。

请你返回 **非空不重叠** 子数组的最大数目，且每个子数组中数字和都为 `target` 。

示例 1：

```
输入：nums = [1,1,1,1,1], target = 2
输出：2
解释：总共有 2 个不重叠子数组（加粗数字表示） [1,1,1,1,1] ，它们的和为目标值 2 。
```

示例 2：

```
输入：nums = [-1,3,5,1,4,2,-9], target = 6
输出：2
解释：总共有 3 个子数组和为 6 。
([5,1], [4,2], [3,5,1,4,2,-9]) 但只有前 2 个是不重叠的。
```

示例 3：

```
输入：nums = [-2,6,6,3,5,4,1,2,8], target = 10
输出：3
```

示例 4：

```
输入：nums = [0,0,0], target = 0
输出：3
```


提示：

- `1 <= nums.length <= 10^5`
- `-10^4 <= nums[i] <= 10^4`
- `0 <= target <= 10^6`

# 思路

这道题贪心的点在于，在向右扩展右边界的过程中，如果此时满足要求，那么一定是选择当前的右边界，而不是继续向右拓展。这样能保证区间的范围最小。

用前缀和 + 哈希表来判断目前的区间是否符合要求。一旦目前存在符合要求的区间便累积答案，清空前缀和和哈希表，从下一位继续开始计算。

前缀和几乎都要配合哈希表来  `O(n)` 地枚举所有符合要求的区间，不要只想到前缀和想不到哈希表呀。

# 代码

```c++
class Solution {
public:
    int maxNonOverlapping(vector<int> &nums, int target) {
        int sum = 0, ans = 0;
        unordered_set<int> hashset;
        hashset.insert(0);
        for (auto &i: nums) {
            sum += i;
            if (hashset.find(sum - target) != hashset.end()) {
                ans++;
                hashset.clear();
                hashset.insert(0);
                sum = 0;
            } else hashset.insert(sum);
        }
        return ans;
    }
};
```

