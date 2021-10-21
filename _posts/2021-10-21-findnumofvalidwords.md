---
layout: post
title: "刷题记录-猜字谜"
subtitle: ""
date: 2021-10-21
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","状态压缩","哈希表"]
---

# 题目

外国友人仿照中国字谜设计了一个英文版猜字谜小游戏，请你来猜猜看吧。

字谜的迷面 puzzle 按字符串形式给出，如果一个单词 word 符合下面两个条件，那么它就可以算作谜底：

单词 word 中包含谜面 puzzle 的第一个字母。
单词 word 中的每一个字母都可以在谜面 puzzle 中找到。
例如，如果字谜的谜面是 "abcdefg"，那么可以作为谜底的单词有 "faced", "cabbage", 和 "baggage"；而 "beefed"（不含字母 "a"）以及 "based"（其中的 "s" 没有出现在谜面中）都不能作为谜底。
返回一个答案数组 answer，数组中的每个元素 answer[i] 是在给出的单词列表 words 中可以作为字谜迷面 puzzles[i] 所对应的谜底的单词数目。

 

示例：

```
输入：
words = ["aaaa","asas","able","ability","actt","actor","access"], 
puzzles = ["aboveyz","abrodyz","abslute","absoryz","actresz","gaswxyz"]
输出：[1,1,3,2,4,0]
解释：
1 个单词可以作为 "aboveyz" 的谜底 : "aaaa" 
1 个单词可以作为 "abrodyz" 的谜底 : "aaaa"
3 个单词可以作为 "abslute" 的谜底 : "aaaa", "asas", "able"
2 个单词可以作为 "absoryz" 的谜底 : "aaaa", "asas"
4 个单词可以作为 "actresz" 的谜底 : "aaaa", "asas", "actt", "access"
没有单词可以作为 "gaswxyz" 的谜底，因为列表中的单词都不含字母 'g'。
```


提示：

- `1 <= words.length <= 10^5`
- `4 <= words[i].length <= 50`
- `1 <= puzzles.length <= 10^4`
- `puzzles[i].length == 7`
- `words[i][j], puzzles[i][j]` 都是小写英文字母。
- 每个 `puzzles[i]` 所包含的字符都不重复。

# 思路

只要求包含，不要求数量，因此可以考虑把每个 word 和 puzzle 处理成 26 个二进制位，每一位表示对应字母是否出现，然后就可以基于 word 构建出现次数的哈希表了。

枚举每个 puzzle 的子串（子串就是随便拿掉几个字母，对应二进制中把一定数量的 0 变成 1），检查子串在哈希表中的出现次数。

枚举 sub-mask 的代码最好背下来。 `for (int sub = mask; sub; sub = (sub - 1) & mask) `

# 代码

```c++
class Solution {
public:
    vector<int> findNumOfValidWords(vector<string> &words, vector<string> &puzzles) {
        int mask, total;
        unordered_map<int, int> freq;
        for (const string &word: words) {
            mask = 0;
            for (char w: word) {
                mask |= (1 << (w - 'a'));
            }
            if (__builtin_popcount(mask))++freq[mask];
        }
        vector<int> ans;

        for (const string &puzzle: puzzles) {
            total = 0, mask = 0;
            for (int i = 1; i < 7; i++) {
                mask |= (1 << (puzzle[i] - 'a'));
            }
            // remove right-most 1 bit
            for (int sub = mask; sub; sub = (sub - 1) & mask) {
                // ensure first bit is 1
                int s = sub | (1 << (puzzle[0] - 'a'));
                if (freq.count(s)) {
                    total += freq[s];
                }
            }
            // special add for word only contains first char of puzzle
            ans.push_back(total + freq[(1 << (puzzle[0] - 'a'))]);
        }
        return ans;
    }
};
```

