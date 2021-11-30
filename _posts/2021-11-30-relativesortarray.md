---
layout: post
title: "刷题记录-数组的相对排序"
subtitle: ""
date: 2021-11-30
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

> https://leetcode-cn.com/problems/relative-sort-array/

# 题目

给你两个数组，`arr1` 和 `arr2`，

`arr2` 中的元素各不相同
`arr2` 中的每个元素都出现在 `arr1` 中
对 `arr1` 中的元素进行排序，使 `arr1` 中项的相对顺序和 `arr2` 中的相对顺序相同。未在 `arr2` 中出现过的元素需要按照升序放在 `arr1` 的末尾。

 

示例：

```
输入：arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
输出：[2,2,2,1,4,3,3,9,6,7,19]
```


提示：

- `1 <= arr1.length, arr2.length <= 1000`
- `0 <= arr1[i], arr2[i] <= 1000`
- `arr2` 中的元素 `arr2[i]` 各不相同
- `arr2` 中的每个元素 `arr2[i]` 都出现在 `arr1` 中

# 思路

自定义排序规则，或者计数排序。

# 代码

```c++
class Solution {
public:
    vector<int> relativeSortArray(vector<int> &arr1, vector<int> &arr2) {
        unordered_set<int> arr2Set;
        for (auto &x: arr2) arr2Set.insert(x);
        vector<int> arr1Absent, arr1Present, arr2IdxToVal(1001);
        for (auto &x: arr1) if (!arr2Set.count(x)) arr1Absent.push_back(x); else arr1Present.push_back(x);
        for (int i = 0; i < arr2.size(); ++i) arr2IdxToVal[arr2[i]] = i;
        sort(arr1Present.begin(), arr1Present.end(),
             [&arr2IdxToVal](auto a, auto b) { return arr2IdxToVal[a] < arr2IdxToVal[b]; });
        sort(arr1Absent.begin(), arr1Absent.end());
        for (auto &x: arr1Absent) arr1Present.push_back(x);
        return arr1Present;
    }
};
```

