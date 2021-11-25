---
layout: post
title: "刷题记录-替换子串得到平衡字符串"
subtitle: ""
date: 2021-11-25
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","滑动窗口"]
---

# 题目

有一个只含有 `'Q', 'W', 'E', 'R'` 四种字符，且长度为 `n` 的字符串。

假如在该字符串中，这四个字符都恰好出现 `n/4` 次，那么它就是一个「平衡字符串」。

 

给你一个这样的字符串 `s`，请通过「替换一个子串」的方式，使原字符串 `s` 变成一个「平衡字符串」。

你可以用和「待替换子串」长度相同的 任何 其他字符串来完成替换。

请返回待替换子串的最小可能长度。

如果原字符串自身就是一个平衡字符串，则返回 0。

 

示例 1：

```
输入：s = "QWER"
输出：0
解释：s 已经是平衡的了。
```

示例 2：

```
输入：s = "QQWE"
输出：1
解释：我们需要把一个 'Q' 替换成 'R'，这样得到的 "RQWE" (或 "QRWE") 是平衡的。
```

示例 3：

```
输入：s = "QQQW"
输出：2
解释：我们可以把前面的 "QQ" 替换成 "ER"。 
```

示例 4：

```
输入：s = "QQQQ"
输出：3
解释：我们可以替换后 3 个 'Q'，使 s = "QWER"。
```


提示：

- `1 <= s.length <= 10^5`
- `s.length` 是 4 的倍数
- `s` 中只含有 `'Q', 'W', 'E', 'R'` 四种字符

# 思路

滑窗，把多出来的部分包在里面，取符合条件的最小的窗口长度就行了。不用管少的字母被包进去了怎么办，反正可以再替换回来。

因为要取符合条件的最小窗口，所以无条件向右扩展，在符合条件时收缩。

# 代码

```c++
class Solution {
public:
    bool check(int *char_cnt, int *window_cnt) {
        for (int i = 0; i < 4; ++i)
            if (window_cnt[i] < char_cnt[i]) return false;
        return true;
    }

    int balancedString(string s) {
        unordered_map<char, int> c_to_idx;
        c_to_idx['Q'] = 0;
        c_to_idx['W'] = 1;
        c_to_idx['E'] = 2;
        c_to_idx['R'] = 3;
        int char_cnt[4] = {0}, window_cnt[4] = {0}, res = INT_MAX;
        for (auto c: s) char_cnt[c_to_idx[c]]++;
        for (int i = 0; i < 4; ++i) char_cnt[i] -= s.length() / 4;
        int start = 0, end = 0;
        while (end < s.length()) {
            window_cnt[c_to_idx[s[end]]]++;
            end++;
            while (check(char_cnt, window_cnt)) {
                res = min(res, end - start);
                window_cnt[c_to_idx[s[start]]]--;
                start++;
            }
        }
        return res;
    }
};
```

