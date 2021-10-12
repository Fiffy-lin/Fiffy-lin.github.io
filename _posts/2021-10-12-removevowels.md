---
layout: post
title: "刷题记录-删去字符串中的元音"
subtitle: ""
date: 2021-10-12
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给你一个字符串 S，请你删去其中的所有元音字母`（ 'a'，'e'，'i'，'o'，'u'）`，并返回这个新字符串。

示例 1：

```
输入："leetcodeisacommunityforcoders"
输出："ltcdscmmntyfrcdrs"
```


示例 2：

```
输入："aeiou"
输出：""
```


提示：

- `S` 仅由小写英文字母组成。
- `1 <= S.length <= 1000`

# 思路

直接模拟。

# 代码

```c++
class Solution {
public:
    string removeVowels(string s) {
        unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
        string res;
        for (auto &c: s)
            if (vowels.find(c) == vowels.end())
                res += c;
        return res;
    }
};
```

