---
layout: post
title: "刷题记录-包含所有三种字符的子字符串数目"
subtitle: ""
date: 2022-02-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","双指针"]
---

> https://leetcode-cn.com/problems/number-of-substrings-containing-all-three-characters/

# 题目

给你一个字符串 `s` ，它只包含三种字符 `a`, `b` 和 `c` 。

请你返回 `a`，`b` 和 `c` 都 至少 出现过一次的子字符串数目。

 

示例 1：

```
输入：s = "abcabc"
输出：10
解释：包含 a，b 和 c 各至少一次的子字符串为 "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" 和 "abc" (相同字符串算多次)。
```

示例 2：

```
输入：s = "aaacb"
输出：3
解释：包含 a，b 和 c 各至少一次的子字符串为 "aaacb", "aacb" 和 "acb" 。
```

示例 3：

```
输入：s = "abc"
输出：1
```


提示：

- 3 <= s.length <= 5 x 10^4
- s 只包含字符 a，b 和 c 。

# 思路

双指针。设满足要求的最小最左区间端点为 `i` `j` ，那么当 `i` 往前移动的时候, `j` 也是单调不减的。因此枚举 `i` ，对每个 `i` 不断向前移动 `j` ，累积答案即可。

# 代码

```c++
class Solution {
    int cnt[3] = {0};
public:
    int numberOfSubstrings(string s) {
        int len = s.length(), ans = 0;
        for (int l = 0, r = 0; l < len; ++l) {
            while (r < len && (cnt[0] < 1 || cnt[1] < 1 || cnt[2] < 1)) {
                cnt[s[r] - 'a']++;
                r++;
            }
            if (!(cnt[0] < 1 || cnt[1] < 1 || cnt[2] < 1))ans += len - r + 1;
            cnt[s[l] - 'a']--;
        }
        return ans;
    }
};
```

