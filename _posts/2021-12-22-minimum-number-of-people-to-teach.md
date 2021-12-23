---
layout: post
title: "刷题记录-需要教语言的最少人数"
subtitle: ""
date: 2021-12-22
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","哈希表","贪心"]
---

> https://leetcode-cn.com/problems/minimum-number-of-people-to-teach/

# 题目

在一个由 `m` 个用户组成的社交网络里，我们获取到一些用户之间的好友关系。两个用户之间可以相互沟通的条件是他们都掌握同一门语言。

给你一个整数 `n` ，数组 `languages` 和数组 `friendships` ，它们的含义如下：

总共有 `n` 种语言，编号从 1 到 `n` 。
`languages[i]` 是第 `i` 位用户掌握的语言集合。
`friendships[i] = [ui, vi]` 表示 `ui` 和 `vi` 为好友关系。
你可以选择 一门 语言并教会一些用户，使得所有好友之间都可以相互沟通。请返回你 最少 需要教会多少名用户。

请注意，好友关系没有传递性，也就是说如果 `x` 和 `y` 是好友，且 `y` 和 `z` 是好友， `x` 和 `z` 不一定是好友。


示例 1：

```
输入：n = 2, languages = [[1],[2],[1,2]], friendships = [[1,2],[1,3],[2,3]]
输出：1
解释：你可以选择教用户 1 第二门语言，也可以选择教用户 2 第一门语言。
```

示例 2：

```
输入：n = 3, languages = [[2],[1,3],[1,2],[3]], friendships = [[1,4],[1,2],[3,4],[2,3]]
输出：2
解释：教用户 1 和用户 3 第三门语言，需要教 2 名用户。
```


提示：

- `2 <= n <= 500`
- `languages.length == m`
- `1 <= m <= 500`
- `1 <= languages[i].length <= n`
- `1 <= languages[i][j] <= n`
- `1 <= ui < vi <= languages.length`
- `1 <= friendships.length <= 500`
- 所有的好友关系 `(ui, vi)` 都是唯一的。
- `languages[i]` 中包含的值互不相同。

# 思路

题目不大好看懂，只能选一门语言教，但是不一定都要用这门语言沟通。

首先排除掉已经可以沟通的，再统计不能沟通的人里面每门语言多少人说。选一个说得最多的教就行了。

# 代码

```c++
class Solution {
public:

    // 判断两个人是否有共同语言
    bool hasCommon(vector<vector<int>>& languages, int i, int j) {
        vector<int> lv1 = languages[i-1];
        vector<int> lv2 = languages[j-1];

        for (auto l1: lv1) {
            for (auto l2: lv2) {
                if (l1 == l2) return true;
            }
        }
        return false;
    }

    int minimumTeachings(int n, vector<vector<int>>& languages, vector<vector<int>>& friendships) {
        unordered_map<int, int> mostLanguage;
        unordered_map<int, int> notConnected;

        // 记录没有共同语言的好友 有哪些人
        for (auto friendship: friendships) {
            if (!hasCommon(languages, friendship[0], friendship[1])) {
                notConnected[friendship[0]]++;
                notConnected[friendship[1]]++;
            }
        }
        // 统计他们会的语言
        for (auto p: notConnected) {
            for (auto language: languages[p.first-1]) {
                mostLanguage[language]++;
            }
        }

        int most = 0;
        // 找到他们里最通用的语言
        for (auto p: mostLanguage) {
            most = max(most, p.second);
        }
        int nodes = notConnected.size();

        // 需要再学习该语言的人数 即他们的总人数-会该语言的人数
        return nodes-most;
    }
};

```

