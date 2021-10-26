---
layout: post
title: "刷题记录-石子游戏 V"
subtitle: ""
date: 2021-10-25
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","DP"]
---

# 题目

几块石子 排成一行 ，每块石子都有一个关联值，关联值为整数，由数组 `stoneValue` 给出。

游戏中的每一轮：Alice 会将这行石子分成两个 非空行（即，左侧行和右侧行）；Bob 负责计算每一行的值，即此行中所有石子的值的总和。Bob 会丢弃值最大的行，Alice 的得分为剩下那行的值（每轮累加）。如果两行的值相等，Bob 让 Alice 决定丢弃哪一行。下一轮从剩下的那一行开始。

只 剩下一块石子 时，游戏结束。Alice 的分数最初为 0 。

返回 Alice 能够获得的最大分数 。 

示例 1：

```
输入：stoneValue = [6,2,3,4,5,5]
输出：18
解释：在第一轮中，Alice 将行划分为 [6，2，3]，[4，5，5] 。左行的值是 11 ，右行的值是 14 。Bob 丢弃了右行，Alice 的分数现在是 11 。
在第二轮中，Alice 将行分成 [6]，[2，3] 。这一次 Bob 扔掉了左行，Alice 的分数变成了 16（11 + 5）。
最后一轮 Alice 只能将行分成 [2]，[3] 。Bob 扔掉右行，Alice 的分数现在是 18（16 + 2）。游戏结束，因为这行只剩下一块石头了。
```

示例 2：

```
输入：stoneValue = [7,7,7,7,7,7,7]
输出：28
```

示例 3：

```
输入：stoneValue = [4]
输出：0
```


提示：

- `1 <= stoneValue.length <= 500`
- `1 <= stoneValue[i] <= 10^6`

# 思路

第一思路是 DP，三维状态转移分别枚举区间长度，左端点以及分割点。

看了题解，有个大佬提出了 `O(n^2)` 的[解法](https://leetcode-cn.com/problems/stone-game-v/solution/on2dong-tai-gui-hua-jie-fa-by-huangyuyang/)

# 代码

```c++
class Solution {
public:
    int stoneGameV(vector<int> &stoneValue) {
        int n = stoneValue.size();
        if (n == 1) return 0;
        if (n == 2) return min(stoneValue[0], stoneValue[1]);
//        vector<vector<int>> dp(n, vector<int>(n, 0));
        int dp[500][500]={0};
        int sumup[501];
//        vector<int> sumup(n + 1);
        for (int i = 1; i <= n; ++i) sumup[i] = sumup[i - 1] + stoneValue[i - 1];
        for (int i = 0; i + 1 < n; ++i) dp[i][i + 1] = min(stoneValue[i], stoneValue[i + 1]);
        for (int length = 2; length < n; length++) {
            for (int i = 0; i + length < n; ++i) {
                for (int split = i; split < i + length; ++split) {
                    int total = sumup[i + length + 1] - sumup[i];
                    int right = sumup[i + length + 1] - sumup[split + 1];
                    int left = total - right;
                    if (left > right) {
                        dp[i][i + length] = max(dp[i][i + length], right + dp[split + 1][i + length]);
                    } else if (left == right) {
                        dp[i][i + length] = max(dp[i][i + length],
                                                max(right + dp[split + 1][i + length], left + dp[i][split]));
                    } else {
                        dp[i][i + length] = max(dp[i][i + length], left + dp[i][split]);
                    }
                }
            }

        }
        return dp[0][n - 1];
    }
};
```

