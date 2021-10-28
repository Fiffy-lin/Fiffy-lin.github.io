---
layout: post
title: "刷题记录-心算挑战"
subtitle: ""
date: 2021-10-28
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","贪心"]
---

# 题目

「力扣挑战赛」心算项目的挑战比赛中，要求选手从 `N` 张卡牌中选出 `cnt` 张卡牌，若这 `cnt` 张卡牌数字总和为偶数，则选手成绩「有效」且得分为 `cnt` 张卡牌数字总和。
给定数组 `cards` 和 `cnt`，其中 `cards[i]` 表示第 `i` 张卡牌上的数字。 请帮参赛选手计算最大的有效得分。若不存在获取有效得分的卡牌方案，则返回 0。

示例 1：

```
输入：cards = [1,2,8,9], cnt = 3

输出：18

解释：选择数字为 1、8、9 的这三张卡牌，此时可获得最大的有效得分 1+8+9=18。
```

示例 2：

```
输入：cards = [3,3,1], cnt = 1

输出：0

解释：不存在获取有效得分的卡牌方案。
```

提示：

- `1 <= cnt <= cards.length <= 10^5`
- `1 <= cards[i] <= 1000`

# 思路

这个数据量几乎没法 DP 了。仔细想想贪心就能搞定。先从大到小选 `cnt` 张，如果是奇数，那就想办法用一张偶数换一张奇数出去，或者用奇数换一张偶数出去。返回两种情况的最大值就行了。

# 代码

```c++
class Solution {
public:
    int maxmiumScore(vector<int> &cards, int cnt) {
        sort(cards.begin(), cards.end(), greater<>());
        int sum = 0, left_odd = 0, left_even = 0, right_odd = 0, right_even = 0, i;
        for (i = 0; i < cnt; ++i) {
            sum += cards[i];
            if (cards[i] & 1) left_odd = cards[i];
            else left_even = cards[i];
        }
        if (!(sum & 1)) return sum;
        for (; i < cards.size(); ++i) {
            if (right_odd && right_even) break;
            if (cards[i] & 1 && !right_odd) right_odd = cards[i];
            else if (!(cards[i] & 1) && !right_even) right_even = cards[i];
        }
        int res1 = 0, res2 = 0;
        if (right_odd && left_even) res1 = sum - left_even + right_odd;
        if (right_even && left_odd) res2 = sum - left_odd + right_even;
        if (res1 && res2) return max(res1, res2);
        else if (res1) return res1;
        else if (res2) return res2;
        else return 0;
    }
};
```

