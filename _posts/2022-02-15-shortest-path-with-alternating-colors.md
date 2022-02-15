---
layout: post
title: "刷题记录-颜色交替的最短路径"
subtitle: ""
date: 2022-02-15
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Medium","BFS"]
---

> https://leetcode-cn.com/problems/shortest-path-with-alternating-colors/

# 题目

在一个有向图中，节点分别标记为 `0, 1, ..., n-1`。这个图中的每条边不是红色就是蓝色，且存在自环或平行边。

`red_edges` 中的每一个 `[i, j]` 对表示从节点 `i` 到节点 `j` 的红色有向边。类似地，`blue_edges` 中的每一个 `[i, j]` 对表示从节点 `i` 到节点 `j` 的蓝色有向边。

返回长度为 `n` 的数组 `answer`，其中 `answer[X]` 是从节点 `0` 到节点 `X` 的红色边和蓝色边交替出现的最短路径的长度。如果不存在这样的路径，那么 `answer[x] = -1`。

 

示例 1：

```
输入：n = 3, red_edges = [[0,1],[1,2]], blue_edges = []
输出：[0,1,-1]
```

示例 2：

```
输入：n = 3, red_edges = [[0,1]], blue_edges = [[2,1]]
输出：[0,1,-1]
```

示例 3：

```
输入：n = 3, red_edges = [[1,0]], blue_edges = [[2,1]]
输出：[0,-1,-1]
```

示例 4：

```
输入：n = 3, red_edges = [[0,1]], blue_edges = [[1,2]]
输出：[0,1,2]
```

示例 5：

```
输入：n = 3, red_edges = [[0,1],[0,2]], blue_edges = [[1,0]]
输出：[0,1,1]
```


提示：

- `1 <= n <= 100`
- `red_edges.length <= 400`
- `blue_edges.length <= 400`
- `red_edges[i].length == blue_edges[i].length == 2`
- `0 <= red_edges[i][j], blue_edges[i][j] < n`

# 思路

求最短路径不仅要想到 Dijkstra 啥的，BFS 也可以。为了满足颜色交替，可以维护每个节点的入边颜色，就可以针对性的在队列里加入另一种颜色的节点。

为了这么做，需要维护两个图和两套 `seen` 数组。

把第一个节点的两种入边都入队作为起始条件。

# 代码

```c++
class Solution {
private:
    vector<vector<int>> red_graph, blue_graph;
    // <number, color>
    // red == 0, blue == 1
    queue<tuple<int, int>> q;
    vector<vector<bool>> seen;
    vector<int> res;
public:
    vector<int> shortestAlternatingPaths(int n, vector<vector<int>> &redEdges, vector<vector<int>> &blueEdges) {
        res.resize(n, INT_MAX);
        red_graph.resize(n, vector<int>(n, 0));
        blue_graph.resize(n, vector<int>(n, 0));
        seen.resize(2, vector<bool>(n, false));
        for (auto pair: redEdges) red_graph[pair[0]].emplace_back(pair[1]);
        for (auto pair: blueEdges) blue_graph[pair[0]].emplace_back(pair[1]);
        q.emplace(0, 0);
        q.emplace(0, 1);
        int distance = 0;
        while (!q.empty()) {
            int sz = q.size();
            for (int i = 0; i < sz; ++i) {
                auto[val, color] = q.front();
                q.pop();
                res[val] = min(res[val], distance);
                if (color == 0) {
                    for (auto dst: blue_graph[val]) {
                        if (!seen[1][dst]) {
                            seen[1][dst] = true;
                            q.emplace(dst, 1);
                        }
                    }
                } else {
                    for (auto dst: red_graph[val]) {
                        if (!seen[0][dst]) {
                            seen[0][dst] = true;
                            q.emplace(dst, 0);
                        }
                    }
                }
            }
            distance++;
        }
        for (auto &x: res) if (x == INT_MAX) x = -1;
        return res;
    }
};
```

