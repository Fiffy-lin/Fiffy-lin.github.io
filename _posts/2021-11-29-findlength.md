---
layout: post
title: "刷题记录-最长重复子数组"
subtitle: ""
date: 2021-11-29
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["DP","滑动窗口"]
---

# 题目

给两个整数数组 `A` 和 `B` ，返回两个数组中公共的、长度最长的子数组的长度。

 

示例：

```
输入：
A: [1,2,3,2,1]
B: [3,2,1,4,7]
输出：3
解释：
长度最长的公共子数组是 [3, 2, 1] 。
```


提示：

- `1 <= len(A), len(B) <= 1000`
- `0 <= A[i], B[i] < 100`

# 思路

- 这个数据量可以用二维 DP。用 `dp[i][j]` 表示两个数组下标 i,j 为结尾的公共长度，则转移方程就很容易写出来。遍历计算每个状态就可以了。
- 也可以用滑窗的思路来做，固定一个数组，滑动另一个的位置，分别计算每个位置的匹配长度。

# 代码

```c++
class Solution {
public:
    int res = 0;

    int findLength(vector<int> &nums1, vector<int> &nums2) {
        int n1 = nums1.size();
        int n2 = nums2.size();
        vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
        for (int i = 1; i <= n1; ++i) {
            for (int j = 1; j <= n2; ++j) {
                dp[i][j] = nums1[i - 1] == nums2[j - 1] ? dp[i - 1][j - 1] + 1 : 0;
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }

    void findMaxLength(vector<int> &nums1, vector<int> &nums2, int start1, int start2, int cmpLength) {
        int cur = 0;
        for (int i = 0; i < cmpLength; ++i) {
            if (nums1[start1 + i] == nums2[start2 + i]) {
                cur++;
                res = max(res, cur);
            } else {
                cur = 0;
            }
        }
    }

    int findLengthSlidingWindow(vector<int> &nums1, vector<int> &nums2) {
        int n1 = nums1.size();
        int n2 = nums2.size();
        for (int i = 0; i < n1; ++i) {
            int cmpLength = min(n1 - i, n2);
            findMaxLength(nums1, nums2, i, 0, cmpLength);
        }
        for (int i = 0; i < n2; ++i) {
            int cmpLength = min(n1, n2 - i);
            findMaxLength(nums1, nums2, 0, i, cmpLength);
        }
        return res;
    }
};
```

