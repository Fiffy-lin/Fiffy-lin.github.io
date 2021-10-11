---
layout: post
title: "刷题记录-至多包含两个不同字符的最长子串"
subtitle: ""
date: 2021-10-11
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","滑动窗口"]
---

# 题目

给定一个字符串 s ，找出 至多 包含两个不同字符的最长子串 t ，并返回该子串的长度。

示例 1:

```
输入: "eceba"
输出: 3
解释: t 是 "ece"，长度为3。
```

示例 2:

```
输入: "ccaabbb"
输出: 5
解释: t 是 "aabbb"，长度为5。
```

# 思路

滑窗，没啥好说的。

# 代码

```c++
class Solution {
public:
    int lengthOfLongestSubstringTwoDistinct(string s) {
        if (s.length() < 3) return s.length();
        vector<int> hashmap(128, 0);
        int start = 0, end = 0, cnt = 0, res = 0;
        while (end < s.length()) {
            if (!hashmap[s[end]]) cnt++;
            hashmap[s[end]]++;
            end++;
            while (cnt == 3) {
                hashmap[s[start]]--;
                if (!hashmap[s[start]]) cnt--;
                start++;
            }
            res = max(res, end - start);
        }
        return res;
    }
};
```

