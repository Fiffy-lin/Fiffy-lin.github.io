---
layout: post
title: "刷题记录-通过删除字母匹配到字典里最长单词"
subtitle: ""
date: 2021-09-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","双指针","DP"]
---

# 题目描述

给你一个字符串 s 和一个字符串数组 dictionary 作为字典，找出并返回字典中最长的字符串，该字符串可以通过删除 s 中的某些字符得到。

如果答案不止一个，返回长度最长且字典序最小的字符串。如果答案不存在，则返回空字符串。

示例 1：

```
输入：s = "abpcplea", dictionary = ["ale","apple","monkey","plea"]
输出："apple"
```

示例 2：

```
输入：s = "abpcplea", dictionary = ["a","b","c"]
输出："a"
```


提示：

- `1 <= s.length <= 1000`
- `1 <= dictionary.length <= 1000`
- `1 <= dictionary[i].length <= 1000`
-  `s `和 `dictionary[i]` 仅由小写英文字母组成

# 思路

### 排序+双指针

最开始的想法就是把题目当成一个子串匹配问题来做，双指针分别指向`s`和当前待匹配的字符串，然后返回完全匹配的最长的串。为了避免维护当前最长&字典序最小的串，可以先把`dictionary`按要求先排好序，在从前往后遍历，找到了就返回。

![](/img/in-post/post-2021-09-14/sort-double-pt.png)

### DP跳表

理解了一下大佬的代码。前一方法的问题在于对`dictionary`中的每个串，我们都需要从前往后线性遍历`s`。因此可以通过维护一个跳表`next`来进行`O(1)`跳转。`next[i][j]`表示从位置`i`开始的下一个`j`字母的下标。因此只需要遍历模式串的字母，并将主串跳至模式串当前字母位置的下一个，直到失配（`next[i][j] == -1`）或完配（`r==m`）。跳表初始化为`-1`和把空串放在`dictionary`中都是为了隐式处理边界条件。

![](/img/in-post/post-2021-09-14/DP.png)

#  代码

### 排序+双指针

```c++
class Solution {
public:
    static bool cmp(string &a, string &b) {
        if (a.length() != b.length())
            return a.length() > b.length();
        else
            return a < b;
    }

    string findLongestWord(string s, vector<string> &dictionary) {
        int idxLen = s.length();
        sort(dictionary.begin(), dictionary.end(), cmp);
        for (auto &str: dictionary) {
            int patLen = str.length();
            int pat = 0, idx = 0;
            while (pat < patLen && idx < idxLen) {
                if (str[pat] == s[idx])
                    pat++;
                idx++;
            }
            if (pat == patLen) return str;
        }
        return "";
    }
};
```

### DP 

[大佬代码传送门](https://leetcode-cn.com/problems/longest-word-in-dictionary-through-deleting/solution/c-zi-ding-yi-pai-xu-dong-tai-gui-hua-yu-mj8jg/)

```c++
class Solution2 {
public:
    string findLongestWord(string s, vector<string> &dic) {
        sort(dic.rbegin(), dic.rend(),
             [](auto &&a, auto &&b) { return a.size() < b.size() || a.size() == b.size() && a > b; });
        dic.push_back("");
        vector<vector<int>> next(s.size() + 1, vector<int>(26, -1));
        for (int i = s.size() - 1; i >= 0; --i)
            for (int j = 0; j < 26; ++j)
                next[i][j] = s[i] == (j + 'a') ? i : next[i + 1][j];
        auto check = [&](string &x) {
            int m = x.size(), l = 0, r = 0;
            while (r < m) if (!(l = next[l][x[r++] - 'a'] + 1)) return false;
            return true;
        };
        return *find_if(begin(dic), end(dic), check);
    }
};
```

