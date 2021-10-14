---
layout: post
title: "刷题记录-删列造序 III"
subtitle: ""
date: 2021-10-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DP"]
---

# 题目

给定由 `N` 个小写字母字符串组成的数组 `A`，其中每个字符串长度相等。

选取一个删除索引序列，对于 `A` 中的每个字符串，删除对应每个索引处的字符。

比如，有 `A = ["babca","bbazb"]`，删除索引序列 `{0, 1, 4}`，删除后 A 为`["bc","az"]`。

假设，我们选择了一组删除索引 `D`，那么在执行删除操作之后，最终得到的数组的行中的每个元素都是按字典序排列的。

清楚起见，`A[0]` 是按字典序排列的（即，`A[0][0] <= A[0][1] <= ... <= A[0][A[0].length - 1]`），`A[1]` 是按字典序排列的（即，`A[1][0] <= A[1][1] <= ... <= A[1][A[1].length - 1]`），依此类推。

请你返回 `D.length` 的最小可能值。

示例 1：

```
输入：["babca","bbazb"]
输出：3
解释：
删除 0、1 和 4 这三列后，最终得到的数组是 A = ["bc", "az"]。
这两行是分别按字典序排列的（即，A[0][0] <= A[0][1] 且 A[1][0] <= A[1][1]）。
注意，A[0] > A[1] —— 数组 A 不一定是按字典序排列的。
```

示例 2：

```
输入：["edcba"]
输出：4
解释：如果删除的列少于 4 列，则剩下的行都不会按字典序排列。
```

示例 3：

```
输入：["ghi","def","abc"]
输出：0
解释：所有行都已按字典序排列。
```


提示：

- `1 <= A.length <= 100`
- `1 <= A[i].length <= 100`

# 思路

删除最少次，让所有字符串都递增，也就是说求最大递增子序列。

想到这个就简单了。

因为是所有串一起删一个位置，所以状态转移的时候要所有串都满足条件才可以。

# 代码

```c++
class Solution {
public:
    int minDeletionSize(vector<string> &strs) {
        vector<int> dp(strs[0].length(), 1);
        for (int i = 1; i < strs[0].length(); ++i) {
            for (int j = 0; j < i; ++j) {
                bool status = true;
                for (auto &str: strs) status &= (str[i] >= str[j]);
                if (status) dp[i] = max(dp[i], dp[j] + 1);
            }
        }
        return strs[0].length() - *max_element(dp.begin(), dp.end());
    }
};
```

