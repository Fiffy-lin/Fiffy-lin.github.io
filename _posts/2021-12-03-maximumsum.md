---
layout: post
title: "刷题记录-删除一次得到子数组最大和"
subtitle: ""
date: 2021-12-03
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DP"]
---

> https://leetcode-cn.com/problems/maximum-subarray-sum-with-one-deletion/

# 题目

给你一个整数数组，返回它的某个 **非空** 子数组（连续元素）在执行一次可选的删除操作后，所能得到的最大元素总和。

换句话说，你可以从原数组中选出一个子数组，并可以决定要不要从中删除一个元素（只能删一次哦），（删除后）子数组中至少应当有一个元素，然后该子数组（剩下）的元素总和是所有子数组之中最大的。

注意，删除一个元素后，子数组 **不能为空**。

请看示例：

示例 1：

```
输入：arr = [1,-2,0,3]
输出：4
解释：我们可以选出 [1, -2, 0, 3]，然后删掉 -2，这样得到 [1, 0, 3]，和最大。
```

示例 2：

```
输入：arr = [1,-2,-2,3]
输出：3
解释：我们直接选出 [3]，这就是最大和。
```

示例 3：

```
输入：arr = [-1,-1,-1,-1]
输出：-1
解释：最后得到的子数组不能为空，所以我们不能选择 [-1] 并从中删去 -1 来得到 0。
     我们应该直接选择 [-1]，或者选择 [-1, -1] 再从中删去一个 -1。
```


提示：

- `1 <= arr.length <= 10^5`
- `-10^4 <= arr[i] <= 10^4`

# 思路

DP，维护当前位置结尾的最大和。由于可以删一次，因此一个位置需要维护两个状态，即没删过和删过。

对于没删过，状态转移相当容易，要么把前一位的算上，要么自己作为一个数组。

对于删过的，要么算上上一位删过的，要么就把上一位删了，算上上上位没删过的。

# 代码

```c++
class Solution {
public:
    int maximumSum(vector<int> &arr) {
        int res = INT_MIN;
        if (arr.size() == 1) return arr[0];
        using isdeleted = pair<int, int>;
        vector<isdeleted> dp(arr.size(), {0, 0});
        dp[0].first = arr[0];
        dp[1].first = max(arr[1] + dp[0].first, arr[1]);
        res = max(dp[0].first, dp[1].first);
        for (int i = 2; i < arr.size(); ++i) {
            dp[i].first = max(arr[i], arr[i] + dp[i - 1].first);
            dp[i].second = max(dp[i - 1].second + arr[i], arr[i] + dp[i - 2].first);
            res = max(max(dp[i].first, dp[i].second), res);
        }
        return res;
    }
};
```

