---
layout: post
title: "刷题记录-最长重复子串"
subtitle: ""
date: 2021-11-22
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二分","哈希"]
---

# 题目

给定字符串 `S`，找出最长重复子串的长度。如果不存在重复子串就返回 0。

示例 1：

```
输入："abcd"
输出：0
解释：没有重复子串。
```

示例 2：

```
输入："abbaba"
输出：2
解释：最长的重复子串为 "ab" 和 "ba"，每个出现 2 次。
```

示例 3：

```
输入："aabcaabdaab"
输出：3
解释：最长的重复子串为 "aab"，出现 3 次。
```

示例 4：

```
输入："aaaaa"
输出：4
解释：最长的重复子串为 "aaaa"，出现 2 次。
```


提示：

- 字符串 `S` 仅包含从 `'a'` 到 `'z'` 的小写英文字母。
- `1 <= S.length <= 1500`

# 思路

一开始的想法是从大到小枚举长度，再枚举起点，通过 `unordered_set` 判断子串的唯一性，超时了。线性枚举超时的时候应该想到二分，对吧？

把线性枚举长度改成二分枚举符合要求的最大长度，此外还可以通过 `Robin-Karp` 方法判断字符串的唯一性，进一步降低第二部分的时间复杂度。

# 代码

```c++
class Solution {
public:
    int find(int length, string &s) {
        unordered_set<string> hashset;
        for (int start = 0; start + length - 1 < s.length(); start++) {
            string sub = s.substr(start, length);
            if (hashset.find(sub) != hashset.end())
                return start;
            hashset.insert(sub);
        }
        return -1;
    }

    int binarySearchMinRepeatingLength(string &s) {
        int left = 0, right = s.length() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (find(mid, s) != -1) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
        return left - 1;
    }

    int longestRepeatingSubstring(string s) {
        int length = binarySearchMinRepeatingLength(s);
        return length == -1 ? 0 : length;
    }
};
```

