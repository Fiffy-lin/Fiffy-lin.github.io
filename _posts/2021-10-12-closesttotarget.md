---
layout: post
title: "刷题记录-找到最接近目标值的函数值"
subtitle: ""
date: 2021-10-12
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/07/19/change.png)

Winston 构造了一个如上所示的函数 `func` 。他有一个整数数组 `arr` 和一个整数 `target` ，他想找到让 `|func(arr, l, r) - target|` 最小的` l` 和 `r` 。

请你返回 `|func(arr, l, r) - target|` 的最小值。

请注意， `func` 的输入参数 `l` 和 `r` 需要满足 `0 <= l, r < arr.length` 。

 

示例 1：

```
输入：arr = [9,12,3,7,15], target = 5
输出：2
解释：所有可能的 [l,r] 数对包括 [[0,0],[1,1],[2,2],[3,3],[4,4],[0,1],[1,2],[2,3],[3,4],[0,2],[1,3],[2,4],[0,3],[1,4],[0,4]]， Winston 得到的相应结果为 [9,12,3,7,15,8,0,3,7,0,0,3,0,0,0] 。最接近 5 的值是 7 和 3，所以最小差值为 2 。
```


示例 2：

```
输入：arr = [1000000,1000000,1000000], target = 1
输出：999999
解释：Winston 输入函数的所有可能 [l,r] 数对得到的函数值都为 1000000 ，所以最小差值为 999999 。
```


示例 3：

```
输入：arr = [1,2,4,8,16], target = 0
输出：0
```


提示：

- `1 <= arr.length <= 10^5`
- `1 <= arr[i] <= 10^6`
- `0 <= target <= 10^7`

# 思路

按位与是单调递减的操作，并且已经变为 0 的位不会再变回 1. 数据范围可以用 `int` 来涵盖，因此按位与的中间结果最多同时只有 31 种。那么按序遍历即可，时间复杂度最大也是`O(31n)`

# 代码

```c++
class Solution {
public:
    int closestToTarget(vector<int> &arr, int target) {
        unordered_set<int> prev;
        prev.insert(arr.front());
        int ans = abs(arr.front() - target);
        for (auto &num: arr) {
            unordered_set<int> curr;
            curr.insert(num);
            ans = min(ans, abs(num - target));
            for (auto &prev_num: prev) {
                int curr_num = prev_num & num;
                ans = min(ans, abs(curr_num - target));
                curr.insert(curr_num);
            }
            prev = curr;
        }
        return ans;
    }
};
```

