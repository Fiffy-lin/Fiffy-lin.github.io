---
layout: post
title: "刷题记录-找出所有行中最小公共元素"
subtitle: ""
date: 2021-12-06
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","二分"]
---

> https://leetcode-cn.com/problems/find-smallest-common-element-in-all-rows/

# 题目

给你一个矩阵 `mat`，其中每一行的元素都已经按 严格递增 顺序排好了。请你帮忙找出在所有这些行中 最小的公共元素。

如果矩阵中没有这样的公共元素，就请返回 -1。

 

示例：

```
输入：mat = [[1,2,3,4,5],[2,4,5,8,10],[3,5,7,9,11],[1,3,5,7,9]]
输出：5
```


提示：

- `1 <= mat.length, mat[i].length <= 500`
- `1 <= mat[i][j] <= 10^4`
- `mat[i]` 已按严格递增顺序排列。

# 思路

做这道题很自然的思路就是把所有的元素都过一遍哈希表，然后从小到大找第一个出现次数等于矩阵行数的数，复杂度 `O(m * n)`

考虑另一种方法，实际上可以对某一个数组里的所有数，二分查找其他数组里是否存在。时间复杂度 `O(m * n * log(n))` 。

参考题解，还有另一种 `O(m * n)` 解法：在所有行中按照有序顺序枚举所有元素，每一行从零开始跟踪当前元素的位置。然后找出所有行跟踪器中较小的元素，并向前移动跟踪器。当所有的位置都指向同一值时，返回该元素。

# 代码

```c++
class Solution {
public:
    int lower_bound(int start, int end, int target, vector<int> &arr) {
        while (start <= end) {
            int mid = start + (end - start) / 2;
            if (arr[mid] >= target) {
                end = mid - 1;
            } else if (arr[mid] < target) start = mid + 1;
        }
        return start;
    }

    int smallestCommonElementBS(vector<vector<int>> &mat) {
        int n = mat.size(), m = mat[0].size();
        vector<int> pos(n);
        for (int j = 0; j < m; ++j) {
            bool found = true;
            for (int i = 1; i < n && found; ++i) {
                pos[i] = lower_bound(pos[i], mat[i].size() - 1, mat[0][j], mat[i]);
                if (pos[i] >= m) {
                    return -1;
                }
                found = mat[i][pos[i]] == mat[0][j];
            }
            if (found) {
                return mat[0][j];
            }
        }
        return -1;
    }

    int smallestCommonElement(vector<vector<int>> &mat) {
        int n = mat.size(), m = mat[0].size(), cur = 0, cnt = 0;
        vector<int> pos(n);
        while (true) {
            for (int i = 0; i < n; ++i) {
                pos[i] = lower_bound(pos[i], mat[i].size() - 1, cur, mat[i]);
                if (pos[i] >= m) return -1;
                if (cur == mat[i][pos[i]]) {
                    if (++cnt == n) return cur;
                } else cnt = 1;
                cur = mat[i][pos[i]];
            }
        }
    }

};
```

