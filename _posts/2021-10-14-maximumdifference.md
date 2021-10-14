---
layout: post
title: "刷题记录-增量元素之间的最大差值"
subtitle: ""
date: 2021-10-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给你一个下标从 0 开始的整数数组 `nums` ，该数组的大小为`n` ，请你计算 `nums[j] - nums[i]` 能求得的 最大差值 ，其中 `0 <= i < j < n` 且` nums[i] < nums[j]` 。

返回 **最大差值** 。如果不存在满足要求的 `i `和 `j` ，返回 `-1 `。

示例 1：

```
输入：nums = [7,1,5,4]
输出：4
解释：
最大差值出现在 i = 1 且 j = 2 时，nums[j] - nums[i] = 5 - 1 = 4 。
注意，尽管 i = 1 且 j = 0 时 ，nums[j] - nums[i] = 7 - 1 = 6 > 4 ，但 i > j 不满足题面要求，所以 6 不是有效的答案。
```

示例 2：

```
输入：nums = [9,4,3,2]
输出：-1
解释：
不存在同时满足 i < j 和 nums[i] < nums[j] 这两个条件的 i, j 组合。
```

示例 3：

```
输入：nums = [1,5,2,10]
输出：9
解释：
最大差值出现在 i = 0 且 j = 3 时，nums[j] - nums[i] = 10 - 1 = 9 。
```


提示：

- `n == nums.length`
- `2 <= n <= 1000`
- `1 <= nums[i] <= 10^9`

# 思路

和前两天做的那个数组间最大距离差不多。逐位处理，记录当前的最小值，遍历到新一位时先计算答案，再更新全局最小值。

# 代码

```c++
class Solution {
public:
    int maximumDifference(vector<int> &nums) {
        vector<int> previous_smallest(nums.size());
        previous_smallest[0] = 0;
        int max_diff = 0, m = 0, n = 0;
        for (int i = 1; i < nums.size(); ++i) {
            if (nums[i] - nums[m] > max_diff) {
                n = i;
                max_diff = nums[i] - nums[m];
            } else if (nums[i] < nums[m]) {
                m = i;
            }
        }
        return max_diff == 0 ? -1 : max_diff;
    }
};
```

