---
layout: post
title: "面试题-按先后顺序返回所有连续子数组的最大值"
subtitle: ""
date: 2021-10-09
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["面试题","Leetcode","滑动窗口","双端队列"]
---

# 题目

给定一个数组 `array` 和一个子数组大小 `k` ，按先后顺序返回 `array` 中所有连续的长度为 `k` 的子数组的最大值。

示例 1：

```
输入: array[] = {1, 2, 3, 1, 4, 5, 2, 3, 6}, k = 3 
输出: 3 3 4 5 5 5 6
Explanation: 
Maximum of 1, 2, 3 is 3
Maximum of 2, 3, 1 is 3
Maximum of 3, 1, 4 is 4
Maximum of 1, 4, 5 is 5
Maximum of 4, 5, 2 is 5 
Maximum of 5, 2, 3 is 5
Maximum of 2, 3, 6 is 6
```

示例 2：

```
输入: array[] = {8, 5, 10, 7, 9, 4, 15, 12, 90, 13}, k = 4 
输出: 10 10 10 15 15 90 90
Explanation:
Maximum of first 4 elements is 10, similarly for next 4 
elements (i.e from index 1 to 4) is 10, So the sequence 
generated is 10 10 10 15 15 90 90
```

# 思路

滑动窗口肯定少不了了。

滑窗 + 大根堆可以做到 `O(n * k)` 的时间复杂度，每次输出堆顶，把该入队元素放在该出队元素位置上，然后重新建堆。

滑窗 + AVL 可以做到 `O(n * log k)` 因为 AVL 的插入和删除都是 `O(log k)`。

滑窗 + 双端队列可以做到 `O(n * k)` ，队列中记录下标。循环遍历中，首先判断队首元素是否该出队，之后把队尾所有小于等于待入队元素的数都从队中拿出（小于当前数的值对计算子数组最大值毫无用处），再把当前数入队。这样队列内的数可以保证严格递减，当前子数组的最大值便始终为队头元素。

# 代码

```c++
class Solution {
public:
    vector<int> maxOfSubArrays(vector<int> array, int k) {
        deque<int> window;
        vector<int> ans;
        for (int i = 0; i < k; ++i) {
            while (!window.empty() && array[window.back()] <= array[i])
                window.pop_back();
            window.push_back(i);
        }
        for (int i = k; i < array.size(); ++i) {
            ans.push_back(array[window.front()]);
            if (window.front() == i - k) window.pop_front();
            while (!window.empty() && array[window.back()] <= array[i])
                window.pop_back();
            window.push_back(i);
        }
        ans.push_back(array[window.front()]);
        return ans;
    }
};
```

