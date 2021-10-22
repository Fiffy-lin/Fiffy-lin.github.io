---
layout: post
title: "刷题记录-摆动排序 II"
subtitle: ""
date: 2021-10-22
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","快速选择","three-way partition","桶排序"]
---

# 题目

给你一个整数数组 `nums`，将它重新排列成 `nums[0] < nums[1] > nums[2] < nums[3]...` 的顺序。

你可以假设所有输入数组都可以得到满足题目要求的结果。

示例 1：

```
输入：nums = [1,5,1,1,6,4]
输出：[1,6,1,5,1,4]
解释：[1,4,1,5,1,6] 同样是符合题目要求的结果，可以被判题程序接受。
```

示例 2：

```
输入：nums = [1,3,2,2,3,1]
输出：[2,3,1,3,1,2]
```


提示：

- `1 <= nums.length <= 5 * 10^4`
- `0 <= nums[i] <= 5000`
- 题目数据保证，对于给定的输入 `nums` ，总能产生满足题目要求的结果

**进阶：**你能用 O(n) 时间复杂度和 / 或原地 O(1) 额外空间来实现吗？

# 思路

最简单的想法是对数组排个序，然后从中间切开，贪心的把前半部分的最大值和后半部分的最大值依次放入。时间复杂度是 `O(nlogn)` 。

为了降低时间复杂度，可以用快速选择找到中位数，three-way partition 来把等于中位数的放中间，小于的放左边，大于的放右边。之后再按照上面的方法切开填入。快速选择可以借助 [std::nth_element()](https://en.cppreference.com/w/cpp/algorithm/nth_element) 来实现。

为了降低空间复杂度，题解说可以用地址映射来避免再建一个排序用的数组。[详见这里](https://leetcode-cn.com/problems/wiggle-sort-ii/solution/yi-bu-yi-bu-jiang-shi-jian-fu-za-du-cong-onlognjia/)

另外，题目的数据范围不大，因此桶排序也能做。

# 代码

```c++
class Solution {
public:
    void wiggleSort(vector<int> &nums) {
        auto midptr = nums.begin() + nums.size() / 2;
        nth_element(nums.begin(), midptr, nums.end());
        int mid = *midptr;

        // 3-way-partition
        int i = 0, j = 0, k = nums.size() - 1;
        while (j < k) {
            if (nums[j] > mid) {
                swap(nums[j], nums[k]);
                --k;
            } else if (nums[j] < mid) {
                swap(nums[j], nums[i]);
                ++i;
                ++j;
            } else {
                ++j;
            }
        }

        if (nums.size() % 2) ++midptr;
        vector<int> tmp1(nums.begin(), midptr);
        vector<int> tmp2(midptr, nums.end());
        for (int i = 0; i < tmp1.size(); ++i) {
            nums[2 * i] = tmp1[tmp1.size() - 1 - i];
        }
        for (int i = 0; i < tmp2.size(); ++i) {
            nums[2 * i + 1] = tmp2[tmp2.size() - 1 - i];
        }
    }
};

class Solution2 {
public:
    void wiggleSort(vector<int> &nums) {
        int length = nums.size();
        int bucket[5001] = {0};
        for (int num: nums) bucket[num]++;
        int big, small;
        if (length & 1) {
            big = length - 2;
            small = length - 1;
        } else {
            big = length - 1;
            small = length - 2;
        }
        int j = 5000;
        for (int i = 1; i <= big; i += 2) {
            while (bucket[j] == 0) j--;
            nums[i] = j;
            bucket[j]--;
        }
        for (int i = 0; i <= small; i += 2) {
            while (bucket[j] == 0) j--;
            nums[i] = j;
            bucket[j]--;
        }
    }
};
```

