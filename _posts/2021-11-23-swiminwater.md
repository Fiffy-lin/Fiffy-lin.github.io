---
layout: post
title: "刷题记录-水位上升的泳池中游泳"
subtitle: ""
date: 2021-11-23
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二分","DFS","BFS","并查集","Dijkstra"]
---

# 题目

在一个 `N x N` 的坐标方格 `grid` 中，每一个方格的值 `grid[i][j]` 表示在位置 `(i,j)` 的平台高度。

现在开始下雨了。当时间为` t `时，此时雨水导致水池中任意位置的水位为 t 。你可以从一个平台游向四周相邻的任意一个平台，但是前提是此时水位必须同时淹没这两个平台。假定你可以瞬间移动无限距离，也就是默认在方格内部游动是不耗时的。当然，在你游泳的时候你必须待在坐标方格里面。

你从坐标方格的左上平台 `(0，0)` 出发。最少耗时多久你才能到达坐标方格的右下平台 `(N-1, N-1)`？

 

示例 1:

```
输入: [[0,2],[1,3]]
输出: 3
解释:
时间为0时，你位于坐标方格的位置为 (0, 0)。
此时你不能游向任意方向，因为四个相邻方向平台的高度都大于当前时间为 0 时的水位。

等时间到达 3 时，你才可以游向平台 (1, 1). 因为此时的水位是 3，坐标方格中的平台没有比水位 3 更高的，所以你可以游向坐标方格中的任意位置
```

示例2:

```
输入: [[0,1,2,3,4],[24,23,22,21,5],[12,13,14,15,16],[11,17,18,19,20],[10,9,8,7,6]]
输出: 16
解释:
 0  1  2  3  4
24 23 22 21  5
12 13 14 15 16
11 17 18 19 20
10  9  8  7  6

最终的路线用加粗进行了标记。
我们必须等到时间为 16，此时才能保证平台 (0, 0) 和 (4, 4) 是连通的
```


提示:

- `2 <= N <= 50`.
- `grid[i][j]` 是 `[0, ..., N*N - 1]` 的排列。

# 思路

遍历时间，判断当前时间从左上角能否到右下角。

遍历时间的过程可以使用二分来优化，判断能不能到可以用 DFS/BFS

还可以用并查集来在线性遍历时间的时候联通对应时刻和周围符合条件的点，并判断起点终点是否联通。

还可以用 Dijkstra，但是我忘干净了。。

# 代码

二分 + DFS/BFS

```c++
class Solution {
public:
    vector<int> X = {1, -1, 0, 0};
    vector<int> Y = {0, 0, 1, -1};
    vector<vector<int>> visited;

    int swimInWater(vector<vector<int>> &grid) {
        int n = grid.size();
        int left = 0, right = n * n - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            visited.clear();
            visited.resize(n, vector<int>(n, 0));
//            if (grid[0][0] <= mid && dfs(mid, 0, 0, n, grid)) {
            if (grid[0][0] <= mid && bfs(mid, n, grid)) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }

    bool is_valid_position(int x, int y, int n) {
        if (x < 0 || x >= n || y < 0 || y >= n) return false;
        return true;
    }

    bool dfs(int time, int x, int y, int n, vector<vector<int>> &grid) {
        visited[x][y] = 1;
        if (x == n - 1 && y == n - 1) return true;
        for (int direction = 0; direction < 4; ++direction) {
            int new_x = x + X[direction];
            int new_y = y + Y[direction];
            if (is_valid_position(new_x, new_y, n) && !visited[new_x][new_y] && grid[new_x][new_y] <= time)
                if (dfs(time, new_x, new_y, n, grid)) return true;
        }
        return false;
    }

    bool bfs(int time, int n, vector<vector<int>> &grid) {
        queue<int> q;
        q.push(0);
        visited[0][0] = 1;
        while (!q.empty()) {
            int front = q.front();
            q.pop();
            int x = front / n;
            int y = front % n;
            if (x == n - 1 && y == n - 1) return true;
            for (int direction = 0; direction < 4; ++direction) {
                int new_x = x + X[direction];
                int new_y = y + Y[direction];
                if (is_valid_position(new_x, new_y, n) && !visited[new_x][new_y] && grid[new_x][new_y] <= time) {
                    q.push(new_x * n + new_y);
                    visited[new_x][new_y] = 1;
                }
            }
        }
        return false;
    }
};
```

并查集：

```c++
class UnionFind {
public:
    vector<int> parent;

    UnionFind(int n) {
        parent.resize(n);
        for (int i = 0; i < n; ++i) parent[i] = i;
    }

    int root(int x) {
        while (x != parent[x]) {
            parent[x] = parent[parent[x]];
            x = parent[x];
        }
        return x;
    }

    void connect(int x, int y) {
        if (is_connected(x, y)) return;
        parent[root(x)] = root(y);
    }

    bool is_connected(int x, int y) {
        return root(x) == root(y);
    }
};

class Solution {
public:
    vector<int> X = {1, -1, 0, 0};
    vector<int> Y = {0, 0, 1, -1};

    bool is_valid_position(int x, int y, int n) {
        if (x < 0 || x >= n || y < 0 || y >= n) return false;
        return true;
    }

    int swimInWater(vector<vector<int>> &grid) {
        int n = grid.size();
        vector<int> time_to_position(n * n);
        UnionFind union_find_set(n * n);
        for (int x = 0; x < n; ++x)
            for (int y = 0; y < n; ++y)
                time_to_position[grid[x][y]] = x * n + y;
        for (int time = 0; time < n * n; ++time) {
            int x = time_to_position[time] / n;
            int y = time_to_position[time] % n;
            for (int direction = 0; direction < 4; ++direction) {
                int new_x = x + X[direction];
                int new_y = y + Y[direction];
                if (is_valid_position(new_x, new_y, n) && grid[new_x][new_y] <= time)
                    union_find_set.connect(time_to_position[time], new_x * n + new_y);
                if (union_find_set.is_connected(0, n * n - 1)) return time;
            }
        }
        return -1;
    }
};
```

