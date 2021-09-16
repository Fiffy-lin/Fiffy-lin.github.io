---
layout: post
title: "刷题记录-单词搜索 II"
subtitle: "前缀树"
date: 2021-09-16
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Trie","DFS"]
---

# 题目描述

给定一个 m x n 二维字符网格 board 和一个单词（字符串）列表 words，找出所有同时在二维网格和字典中出现的单词。

单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

示例 1：

 ![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

```
输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
输出：["eat","oath"]
```

示例 2：

```
输入：board = [["a","b"],["c","d"]], words = ["abcb"]
输出：[]
```


提示：

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 12`
- `board[i][j] `是一个小写英文字母
- `1 <= words.length <= 3 * 104`
- `1 <= words[i].length <= 10`
- `words[i] `由小写英文字母组成
- `words` 中的所有字符串互不相同

# 思路

最初的思路就是暴力搜索。注意到 `1 <= words[i].length <= 10`，因此 DFS 深度大于 10 就可以直接剪掉。时间复杂度 `O(m * n * 4^10)`

考虑优化方法，使用前缀树来避免无意义的递归搜索。先用`words`生成前缀树，只有存在符合要求的路径再往下递归。

我的 `DFS` 代码习惯是进入下层递归前判断条件是否合法，那么要注意第一次调用递归时也要手动判断，因为这里存在前缀树，递归起点不一定是合法的。（没有以起点字母开头的单词。）另外也要注意前缀树节点在本层递归内的含义。一开始传入 `root` ，则要在`DFS`开头让前缀树节点前进一格。

# 代码

```c++
class TrieNode {
public:
    TrieNode *next[27];
    string word;

    TrieNode() {
        memset(next, 0, sizeof(next));
        word = "";
    }

    static void add(TrieNode *insert_pos, string &word, int idx) {
        if (idx == word.length()) insert_pos->word = word;
        else {
            int char_idx = word[idx] - 'a';
            if (!insert_pos->next[char_idx]) insert_pos->next[char_idx] = new TrieNode();
            add(insert_pos->next[char_idx], word, idx + 1);
        }
    }
};

class Solution {
    TrieNode *root = new TrieNode();
    int X[4] = {1, -1, 0, 0};
    int Y[4] = {0, 0, 1, -1};
    vector<string> result;
public:
    bool judge(vector<vector<char>> &board, int i, int j, int m, int n, TrieNode *cur) {
        if (i < 0 || i >= m || j < 0 || j >= n) return false;
        if (board[i][j] == '.' || !cur->next[board[i][j] - 'a']) return false;
        return true;
    }

    void dfs(TrieNode *cur, vector<vector<char>> &board, int i, int j, int m, int n) {
        cur = cur->next[board[i][j] - 'a'];
        if (cur->word != "") {
            result.push_back(cur->word);
            cur->word = "";
        }
        char c = board[i][j];
        board[i][j] = '.';
        for (int direction = 0; direction < 4; ++direction) {
            int newi = i + X[direction], newj = j + Y[direction];
            if (judge(board, newi, newj, m, n, cur)) {
                dfs(cur, board, newi, newj, m, n);
            }
        }
        board[i][j] = c;
    }

    vector<string> findWords(vector<vector<char>> &board, vector<string> &words) {
        for (auto &word: words) TrieNode::add(root, word, 0);
        int m = board.size(), n = board[0].size();
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (root->next[board[i][j] - 'a'])
                    dfs(root, board, i, j, m, n);
        return result;
    }
};
```

