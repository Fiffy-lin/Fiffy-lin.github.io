---
layout: post
title: "刷题记录-俄罗斯套娃信封问题"
subtitle: ""
date: 2022-02-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","DP","二分"]
---

> https://leetcode-cn.com/problems/russian-doll-envelopes/

# 题目

给你一个二维整数数组 `envelopes` ，其中 `envelopes[i] = [wi, hi]` ，表示第 i 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 最多能有多少个 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

注意：不允许旋转信封。


示例 1：

```
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```

示例 2：

```
输入：envelopes = [[1,1],[1,1],[1,1]]
输出：1
```


提示：

- `1 <= envelopes.length <= 10^5`
- `envelopes[i].length == 2`
- `1 <= wi, hi <= 10^5`

# 思路

排序后 LIS，因为两个维度都不能相等，所以可以把第二维度倒着排，就不会出现 `[[1,1], [1,2]]` 套在一起的情况了。

超时了，下面抄题解：

用 `f[i]` 表示长度为 `i` 的递增子序列末尾元素最小值。遍历当前数字 `j` , 如果比最长的队列末尾值还小，那么就在队列里增加 `f[i+1] = j` 这一项。如果不，则在序列里找一个小的接在后面。（为啥需要这一步，因为前面排序的时候把第二维度倒着排了。因此可能出现第一维度相等，第二维度可以让答案更小的情况，这时候需要更新。）

# 代码

```c++
class Solution {
public:
    int maxEnvelopes(vector<vector<int>>& envelopes) {
        if (envelopes.empty()) {
            return 0;
        }
        
        int n = envelopes.size();
        sort(envelopes.begin(), envelopes.end(), [](const auto& e1, const auto& e2) {
            return e1[0] < e2[0] || (e1[0] == e2[0] && e1[1] > e2[1]);
        });

        vector<int> f = {envelopes[0][1]};
        for (int i = 1; i < n; ++i) {
            if (int num = envelopes[i][1]; num > f.back()) {
                f.push_back(num);
            }
            else {
                auto it = lower_bound(f.begin(), f.end(), num);
                *it = num;
            }
        }
        return f.size();
    }
};
```

