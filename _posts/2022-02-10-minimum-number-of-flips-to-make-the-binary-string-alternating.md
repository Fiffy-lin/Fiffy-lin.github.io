---
layout: post
title: "刷题记录-使二进制字符串字符交替的最少反转次数"
subtitle: ""
date: 2022-02-10
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","滑动窗口"]
---

> https://leetcode-cn.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/

# 题目

给你一个二进制字符串 `s` 。你可以按任意顺序执行以下两种操作任意次：

- 类型 1 ：删除 字符串 `s` 的第一个字符并将它 添加 到字符串结尾。
- 类型 2 ：选择 字符串 `s` 中任意一个字符并将该字符 反转 ，也就是如果值为 '0' ，则反转得到 '1' ，反之亦然。

请你返回使 s 变成 交替 字符串的前提下， 类型 2 的 最少 操作次数 。

我们称一个字符串是 交替 的，需要满足任意相邻字符都不同。

比方说，字符串 "010" 和 "1010" 都是交替的，但是字符串 "0100" 不是。


示例 1：

```
输入：s = "111000"
输出：2
解释：执行第一种操作两次，得到 s = "100011" 。
然后对第三个和第六个字符执行第二种操作，得到 s = "101010" 。
```

示例 2：

```
输入：s = "010"
输出：0
解释：字符串已经是交替的。
```

示例 3：

```
输入：s = "1110"
输出：1
解释：对第二个字符执行第二种操作，得到 s = "1010" 。
```


提示：

- `1 <= s.length <= 10^5`
- `s[i]` 要么是 `'0'` ，要么是 `'1'` 。

# 思路

操作 1 的实现不必真的删除添加，我们可以复制一份原来的字符串放到后面，就可以利用滑动窗口。

我们可以先算把原来的字符串变成 01010101... 这种串需要的操作次数 `cnt`，如果要变成 10101010... 就可以直接用 `len - cnt` 得到。

遍历完整个窗口就可以得到结果。

# 代码

```c++
class Solution {
public:
    int minFlips(string s) {
        int len = s.length(), cnt = 0, ans = INT_MAX;
        string target = "01";
        for (int i = 0; i < len; ++i) cnt += (s[i] != target[i % 2]);
        ans = min(cnt, len - cnt);
        for (int i = 0; i < len; ++i) {
            cnt -= (s[i] != target[i % 2]);
            cnt += (s[i] != target[(i + len)  % 2]);
            ans = min(ans, min(cnt, len - cnt));
        }
        return ans;
    }
};
```

