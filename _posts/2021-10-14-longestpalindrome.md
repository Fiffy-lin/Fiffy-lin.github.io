---
layout: post
title: "刷题记录-最长回文串"
subtitle: ""
date: 2021-10-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

注意:
假设字符串的长度不会超过 1010。

示例 1:

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```



# 思路

偶数全用上，奇数最长的全用上，其余少用一个。

# 代码

```c++
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char, int> cnt;
        for (auto &c: s)
            cnt[c]++;
        int has_even = 0;
        int ans = 0;
        for (auto &[c, t]: cnt) {
            if (t & 1) {
                ans += t - 1;
                has_even = 1;
            } else ans += t;
        }
        return ans + has_even;
    }
};
```

