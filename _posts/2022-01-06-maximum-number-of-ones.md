---
layout: post
title: "刷题记录-矩阵中 1 的最大数量"
subtitle: ""
date: 2022-01-06
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","贪心"]
---

> https://leetcode-cn.com/problems/maximum-number-of-ones/

# 题目

现在有一个尺寸为 `width * height` 的矩阵 `M`，矩阵中的每个单元格的值不是 0 就是 1。

而且矩阵 M 中每个大小为 `sideLength * sideLength` 的 正方形 子阵中，1 的数量不得超过 `maxOnes`。

请你设计一个算法，计算矩阵中最多可以有多少个 1。

 

示例 1：

```
输入：width = 3, height = 3, sideLength = 2, maxOnes = 1
输出：4
解释：
题目要求：在一个 3*3 的矩阵中，每一个 2*2 的子阵中的 1 的数目不超过 1 个。
最好的解决方案中，矩阵 M 里最多可以有 4 个 1，如下所示：
[1,0,1]
[0,0,0]
[1,0,1]
```

示例 2：

```
输入：width = 3, height = 3, sideLength = 2, maxOnes = 2
输出：6
解释：
[1,0,1]
[1,0,1]
[1,0,1]
```


提示：

- `1 <= width, height <= 100`
- `1 <= sideLength <= width, height`
- `0 <= maxOnes <= sideLength * sideLength`

# 思路

既然规定了一个 `sideLength * sideLength` 格子里面的上限，我们遍历这个格子里面的每个点，算一下如果选了这个点，对整个大区域能放上几个点（大区域看成由 n 个小格子拼接而成）。然后对结果排序，选择前 `maxOnes` 个点，加起来就是所求的最大值。

# 代码

```
class Solution {
public:
    int maximumNumberOfOnes(int width, int height, int sideLength, int maxOnes) {
        vector<int> data;
        for (int i = 0; i < sideLength; ++i)
            for (int j = 0; j < sideLength; ++j)
                data.push_back(ceil((double) (width - i) / sideLength) * ceil((double) (height - j) / sideLength));
        sort(data.begin(), data.end(), greater<int>());
        int result = 0;
        for (int i = 0; i < maxOnes; ++i)
            result += data[i];
        return result;
    }
};
```

