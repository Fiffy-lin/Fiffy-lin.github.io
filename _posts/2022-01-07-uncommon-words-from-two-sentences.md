---
layout: post
title: "刷题记录-两句话中的不常见单词"
subtitle: ""
date: 2022-01-07
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Easy","哈希表"]
---

> https://leetcode-cn.com/problems/uncommon-words-from-two-sentences/

# 题目

句子 是一串由空格分隔的单词。每个 单词 仅由小写字母组成。

如果某个单词在其中一个句子中恰好出现一次，在另一个句子中却 没有出现 ，那么这个单词就是 不常见的 。

给你两个 句子 `s1` 和 `s2` ，返回所有 不常用单词 的列表。返回列表中单词可以按 任意顺序 组织。

 

示例 1：

```
输入：s1 = "this apple is sweet", s2 = "this apple is sour"
输出：["sweet","sour"]
```

示例 2：

```
输入：s1 = "apple apple", s2 = "banana"
输出：["banana"]
```


提示：

- `1 <= s1.length, s2.length <= 200`
- `s1` 和 `s2` 由小写英文字母和空格组成
- `s1` 和 `s2` 都不含前导或尾随空格
- `s1` 和 `s2` 中的所有单词间均由单个空格分隔

# 思路

哈希表过一遍每个词，只有仅出现一次的词符合要求。

# 代码

```c++
class Solution {
public:
    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int> wordcnt;
        vector<string> res;
        int pos = 0;
        while ((pos = s1.find(" ")) != string::npos) {
            wordcnt[(s1.substr(0, pos))]++;
            s1.erase(0, pos + 1);
        }
        wordcnt[s1]++;
        pos = 0;
        while ((pos = s2.find(" ")) != string::npos) {
            wordcnt[(s2.substr(0, pos))]++;
            s2.erase(0, pos + 1);
        }
        wordcnt[s2]++;
        for (auto &[word, cnt]: wordcnt) {
            if (cnt == 1) res.push_back(word);
        }
        return res;
    }
};
```

