---
layout: post
title: "刷题记录-判断单词是否能放入填字游戏内"
subtitle: ""
date: 2021-10-26
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode"]
---

# 题目

给你一个 `m x n` 的矩阵 `board` ，它代表一个填字游戏 当前 的状态。填字游戏格子中包含小写英文字母（已填入的单词），表示 空 格的 `' '` 和表示 障碍 格子的 `'#'` 。

如果满足以下条件，那么我们可以 水平 （从左到右 或者 从右到左）或 竖直 （从上到下 或者 从下到上）填入一个单词：

该单词不占据任何 `'#'` 对应的格子。
每个字母对应的格子要么是 `' '` （空格）要么与 `board` 中已有字母 匹配 。
如果单词是 水平 放置的，那么该单词左边和右边 相邻 格子不能为 `' '` 或小写英文字母。
如果单词是 竖直 放置的，那么该单词上边和下边 相邻 格子不能为 `' '` 或小写英文字母。
给你一个字符串 `word` ，如果 `word` 可以被放入 `board` 中，请你返回 `true` ，否则请返回 `false` 。

示例 1：

![](https://assets.leetcode.com/uploads/2021/09/18/crossword-1.png)

```
输入：board = [["#", " ", "#"], [" ", " ", "#"], ["#", "c", " "]], word = "abc"
输出：true
解释：单词 "abc" 可以如上图放置（从上往下）。
```

示例 2：

![](https://assets.leetcode.com/uploads/2021/09/18/c2.png)

```
输入：board = [[" ", "#", "a"], [" ", "#", "c"], [" ", "#", "a"]], word = "ac"
输出：false
解释：无法放置单词，因为放置该单词后上方或者下方相邻格会有空格。
```

示例 3：

![](https://assets.leetcode.com/uploads/2021/09/18/crossword-2.png)

```
输入：board = [["#", " ", "#"], [" ", " ", "#"], ["#", " ", "c"]], word = "ca"
输出：true
解释：单词 "ca" 可以如上图放置（从右到左）。
```


提示：

- `m == board.length`
- `n == board[i].length`
- `1 <= m * n <= 2 * 10^5`
- `board[i][j]` 可能为 `' '` ，`'#'` 或者一个小写英文字母。
- `1 <= word.length <= max(m, n)`
- `word` 只包含小写英文字母。

# 思路

枚举每一个起始位置，逐个检查是否符合条件。

# 代码

```c++
class Solution {
public:
    bool placeWordInCrossword(vector<vector<char>> &board, string word) {
        int X[] = {0, 0, 1, -1}, Y[] = {1, -1, 0, 0};
        int m = board.size(), n = board.front().size();
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                // this position is not #
                if (board[i][j] == '#') continue;
                for (int direction = 0; direction < 4; ++direction) {
                    // prev position is bound or #
                    int x_prev = i - X[direction], y_prev = j - Y[direction];
                    if (x_prev >= 0 && x_prev < m && y_prev >= 0 && y_prev < n)
                        if (board[x_prev][y_prev] != '#') continue;
                    // from here board can contain this word
                    int l = word.length();
                    int x_post = i + l * X[direction], y_post = j + l * Y[direction];
                    if (x_post < -1 || x_post > m || y_post < -1 || y_post > n) continue;
                    // post position is bound or #
                    if (x_post >= 0 && x_post < m && y_post >= 0 && y_post < n)
                        if (board[x_post][y_post] != '#') continue;
                    // position with char should match word
                    bool flag = true;
                    for (int k = 0; k < l; ++k) {
                        int x = i + k * X[direction], y = j + k * Y[direction];
                        if (board[x][y] == '#') flag = false;
                        else if (board[x][y] != ' ' && board[x][y] != word[k]) flag = false;
                    }
                    if (flag) return true;
                }
            }
        }
        return false;
    }
};
```

