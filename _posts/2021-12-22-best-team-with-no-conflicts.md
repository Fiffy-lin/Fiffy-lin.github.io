---
layout: post
title: "刷题记录-无矛盾的最佳球队"
subtitle: ""
date: 2021-12-22
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","DP"]
---

> https://leetcode-cn.com/problems/best-team-with-no-conflicts/

# 题目

假设你是球队的经理。对于即将到来的锦标赛，你想组合一支总体得分最高的球队。球队的得分是球队中所有球员的分数 总和 。

然而，球队中的矛盾会限制球员的发挥，所以必须选出一支 没有矛盾 的球队。如果一名年龄较小球员的分数 严格大于 一名年龄较大的球员，则存在矛盾。同龄球员之间不会发生矛盾。

给你两个列表 `scores` 和 `ages`，其中每组 `scores[i]` 和 `ages[i]` 表示第 `i` 名球员的分数和年龄。请你返回 所有可能的无矛盾球队中得分最高那支的分数 。

 

示例 1：

```
输入：scores = [1,3,5,10,15], ages = [1,2,3,4,5]
输出：34
解释：你可以选中所有球员。
```

示例 2：

```
输入：scores = [4,5,6,5], ages = [2,1,2,1]
输出：16
解释：最佳的选择是后 3 名球员。注意，你可以选中多个同龄球员。
```

示例 3：

```
输入：scores = [1,2,3,5], ages = [8,9,10,1]
输出：6
解释：最佳的选择是前 3 名球员。
```


提示：

- `1 <= scores.length, ages.length <= 1000`
- `scores.length == ages.length`
- `1 <= scores[i] <= 10^6`
- `1 <= ages[i] <= 1000`

# 思路

这个数据量基本告别回溯了，考虑 DP。

先按年龄排序，用 `dp[i]` 表示以排序后的 `i` 名球员结尾的球队能拿到的最高分数。状态转移的时候，只考虑上一位球员的分和当前球员的分就行了。如果比上一位高就一定能加入，不会存在比上一位高但比前面某位低的情况，不然上一位都进不来。

# 代码

```c++
class Solution {
public:
    int bestTeamScore(vector<int> &scores, vector<int> &ages) {
        int n = scores.size();
        vector<int> order(n);
        for (int i = 0; i < n; ++i)
            order[i] = i;
        sort(order.begin(), order.end(), [&](int i, int j) {
            return ages[i] < ages[j] || (ages[i] == ages[j] && scores[i] < scores[j]);
        });
        vector<int> dp(n);
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            int idx = order[i];
            dp[i] = scores[idx];
            for (int j = 0; j < i; ++j) {
                int last = order[j];
                if (scores[last] <= scores[idx])
                    dp[i] = max(dp[i], dp[j] + scores[idx]);
            }
            ans = max(ans, dp[i]);
        }
        return ans;
    }
};
```

