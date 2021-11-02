---
layout: post
title: "刷题记录-找出第 k 小的距离对"
subtitle: ""
date: 2021-11-01
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二分","双指针"]
---

# 题目

给定一个整数数组，返回所有数对之间的第 `k` 个最小距离。一对` (A, B)` 的距离被定义为 `A` 和 `B` 之间的绝对差值。

示例 1:

```
输入：
nums = [1,3,1]
k = 1
输出：0 
解释：
所有数对如下：
(1,3) -> 2
(1,1) -> 0
(3,1) -> 2
因此第 1 个最小距离的数对是 (1,1)，它们之间的距离为 0。
```

提示:

- `2 <= len(nums) <= 10000`
- `0 <= nums[i] < 1000000`
- `1 <= k <= len(nums) * (len(nums) - 1) / 2`

# 思路

这个数据量不大好使用堆，复杂度太高了。

先排序，复杂度是 `O(nlog n)` ，再考虑二分查找符合要求的距离。上界为最大元素和最小元素间差值，下界为 0. 判断收缩边界的条件是对于当前距离，是否有至少 k 个数比这个距离小。检查这一条件可以用双指针在 `O(n)` 的时间复杂度下完成。

# 代码

```c++
class Solution {
public:
    bool check(int dist, vector<int> &nums, int k) {
        int i = 0, j = 1, cnt = 0;
        for (; i < nums.size(); ++i) {
            while (j < nums.size() && nums[i] + dist >= nums[j]) j++;
            cnt += j - i - 1;
        }
        return cnt >= k;
    }

    int smallestDistancePair(vector<int> &nums, int k) {
        sort(nums.begin(), nums.end());
        int n = nums.size(), l = 0, r = nums[n - 1] - nums[0], ans = 0;
        while (l <= r) {
            int mid = (l + r) / 2;
            if (check(mid, nums, k)) {
                ans = mid;
                r = mid - 1;
            } else l = mid + 1;
        }
        return ans;
    }
};
```

