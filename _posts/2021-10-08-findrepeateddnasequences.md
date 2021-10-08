---
layout: post
title: "刷题记录-重复的DNA序列"
subtitle: ""
date: 2021-10-08
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","哈希表","位运算","滑动窗口"]
---

# 题目

所有 DNA 都由一系列缩写为 `'A'`，`'C'`，`'G'` 和 `'T'` 的核苷酸组成，例如：`"ACGAATTCCG"`。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来找出所有目标子串，目标子串的长度为 10，且在 DNA 字符串 `s` 中出现次数超过一次。

示例 1：

```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```

示例 2：

```
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```


提示：

- `0 <= s.length <= 10^5`
- `s[i]` 为 `'A'`、`'C'`、`'G'` 或 `'T'`

# 思路

先暴力试一试。线性枚举字符串中的所有子串并统计次数，能过。

前一个方法主要问题是耗时和耗内存太大，耗时是因为`string::substr()` 操作具有 `O(n)` 时间复杂度，耗内存是因为哈希表的 `key` 是长度为 10 的 `string` 。由于只有四种字母，长度为 10，考虑用 10 个 2 位二进制共 20 位来表示每个串，把时间复杂度也优化到了 `O(n)` 。因此遍历过程变成了：

1. 前移两位 `cur = cur << 2;`
2. 加上此位 `cur = cur | 本位的二进制表示` 
3. 去掉开头位 `cur = cur & ((1 << 20) - 1);`
4. 判断是否为合法答案……

初始化过程可以只遍历前 9 个数，这样可以少写一点特判代码。

# 代码

```c++
class Solution {
public:
    vector<string> findRepeatedDnaSequencesBruteForce(string s) {
        int len = s.length();
        unordered_map<string, int> hashmap;
        vector<string> ans;
        for (int i = 0; i < len - 10 + 1; ++i) {
            string sub = s.substr(i, 10);
            hashmap[sub]++;
            if (hashmap[sub] == 2)
                ans.push_back(sub);
        }
        return ans;
    }

    vector<string> findRepeatedDnaSequences(string s) {
        int len = s.length();
        unordered_map<char, int> char_to_id;
        char_to_id['A'] = 0;
        char_to_id['C'] = 1;
        char_to_id['G'] = 2;
        char_to_id['T'] = 3;
        unordered_map<int, int> hashmap;
        vector<string> ans;
        int cur = 0;
        for (int i = 0; i < 9; ++i)
            cur = (cur << 2) | char_to_id[s[i]];
        for (int i = 0; i < len - 10 + 1; ++i) {
            cur = ((cur << 2) | char_to_id[s[i + 10 - 1]]) & ((1 << 20) - 1);
            hashmap[cur]++;
            if (hashmap[cur] == 2)
                ans.push_back(s.substr(i, 10));
        }
        return ans;
    }
};
```

