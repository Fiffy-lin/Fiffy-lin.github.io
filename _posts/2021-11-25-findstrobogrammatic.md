---
layout: post
title: "刷题记录-中心对称数 II"
subtitle: ""
date: 2021-11-25
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","递归"]
---

# 题目

中心对称数是指一个数字在旋转了 180 度之后看起来依旧相同的数字（或者上下颠倒地看）。

找到所有长度为 n 的中心对称数。

示例 :

```
输入:  n = 2
输出: ["11","69","88","96"]
```

# 思路

一位的对称数只有 0,1,8

二位的有 11, 69, 88, 96，如果两侧有其他数，还可以有 00

往里加就完事了

# 代码

```c++
class Solution {
public:
    vector<string> helper(int n, bool first = false) {
        if (n == 0) {
            return vector<string>{""};
        }
        if (n == 1) {
            return vector<string>{"0", "1", "8"};
        }
        vector<string> subs = helper(n - 2);
        vector<string> ans;
        for (auto sub: subs) {
            if (!first) {
                ans.push_back("0" + sub + "0");
            }
            ans.push_back("1" + sub + "1");
            ans.push_back("6" + sub + "9");
            ans.push_back("8" + sub + "8");
            ans.push_back("9" + sub + "6");
        }
        return ans;
    }

    vector<string> findStrobogrammatic(int n) {
        return helper(n, true);
    }
};
```

