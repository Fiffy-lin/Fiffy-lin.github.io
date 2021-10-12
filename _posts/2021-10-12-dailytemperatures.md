---
layout: post
title: "刷题记录-每日温度"
subtitle: ""
date: 2021-10-12
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","单调栈"]
---

# 题目

请根据每日 `气温` 列表 `temperatures` ，请计算在每一天需要等几天才会有更高的温度。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

示例 1:

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```


示例 2:

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

示例 3:

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```


提示：

- `1 <= temperatures.length <= 10^5`
- `30 <= temperatures[i] <= 100`

# 思路

标准的单调栈。题目要求更高温的日期，那就当前温度比栈顶高就出栈，同时更新结果。

# 代码

```c++
class Solution {
public:
    vector<int> dailyTemperatures(vector<int> &temperatures) {
        deque<int> monotone_stack;
        vector<int> res(temperatures.size());
        for (int i = 0; i < temperatures.size(); ++i) {
            while (!monotone_stack.empty() && temperatures[i] > temperatures[monotone_stack.back()]) {
                res[monotone_stack.back()] = i - monotone_stack.back();
                monotone_stack.pop_back();
            }
            monotone_stack.push_back(i);
        }
        while (!monotone_stack.empty()) {
            res[monotone_stack.back()] = 0;
            monotone_stack.pop_back();
        }
        return res;
    }
};
```

