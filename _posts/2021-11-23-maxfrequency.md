---
layout: post
title: "刷题记录-最高频元素的频数"
subtitle: ""
date: 2021-11-23
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","滑动窗口"]
---

# 题目

元素的 频数 是该元素在一个数组中出现的次数。

给你一个整数数组 `nums` 和一个整数 `k` 。在一步操作中，你可以选择 `nums` 的一个下标，并将该下标对应元素的值增加 1 。

执行最多 `k` 次操作后，返回数组中最高频元素的 最大可能频数 。

 

示例 1：

```
输入：nums = [1,2,4], k = 5
输出：3
解释：对第一个元素执行 3 次递增操作，对第二个元素执 2 次递增操作，此时 nums = [4,4,4] 。
4 是数组中最高频元素，频数是 3 。
```

示例 2：

```
输入：nums = [1,4,8,13], k = 5
输出：2
解释：存在多种最优解决方案：

对第一个元素执行 3 次递增操作，此时 nums = [4,4,8,13] 。4 是数组中最高频元素，频数是 2 。

对第二个元素执行 4 次递增操作，此时 nums = [1,8,8,13] 。8 是数组中最高频元素，频数是 2 。

对第三个元素执行 5 次递增操作，此时 nums = [1,4,13,13] 。13 是数组中最高频元素，频数是 2 。
```

示例 3：

```
输入：nums = [3,9,6], k = 2
输出：1
```


提示：

- `1 <= nums.length <= 10^5`
- `1 <= nums[i] <= 10^5`
- `1 <= k <= 10^5`

# 思路

首先，这个最大频次的数一定是数组中出现的数。不然我们一定可以少加几次，让这个数变成比他小的第一个出现的数。

我们先排序，然后观察一下把当前都加好的同频数统一加到下一位数的过程。

![](https://pic.leetcode-cn.com/1626653620-uyAtsn-123131.png)

可以看出来，`r` 向右扩展一位的过程中，总操作数增加了 `(right - left) * (nums[right] - nums[right - 1])` ，如果操作数不够了，我们就把最左边的数放弃掉，不加它了，就可以多出来 `nums[right] - nums[left]` 这么多操作数。

# 代码

```c++
class Solution {
public:
    int maxFrequency(vector<int> &nums, int k) {
        sort(nums.begin(), nums.end());
        int left = 0, ans = 1;
        long long total_ops = 0;
        for (int right = 1; right < nums.size(); ++right) {
            total_ops += (long long) (right - left) * (nums[right] - nums[right - 1]);
            if (total_ops > k) {
                total_ops -= nums[right] - nums[left];
                left++;
            }
            ans = max(ans, right - left + 1);
        }
        return ans;
    }
};
```

