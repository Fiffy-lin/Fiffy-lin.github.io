---
layout: post
title: "刷题记录-统计农场中肥沃金字塔的数目"
subtitle: ""
date: 2021-12-21
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","DP"]
---

> https://leetcode-cn.com/problems/count-fertile-pyramids-in-a-land/

# 题目

有一个 **矩形网格** 状的农场，划分为 `m` 行 `n` 列的单元格。每个格子要么是 肥沃的 （用 1 表示），要么是 贫瘠 的（用 0 表示）。网格图以外的所有与格子都视为贫瘠的。

农场中的 金字塔 区域定义如下：

区域内格子数目 大于 1 且所有格子都是 肥沃的 。
金字塔 顶端 是这个金字塔 最上方 的格子。金字塔的高度是它所覆盖的行数。令 `(r, c)` 为金字塔的顶端且高度为 h ，那么金字塔区域内包含的任一格子 `(i, j)` 需满足 `r <= i <= r + h - 1` 且 `c - (i - r) <= j <= c + (i - r)` 。
一个 倒金字塔 类似定义如下：

区域内格子数目 大于 1 且所有格子都是 肥沃的 。
倒金字塔的 顶端 是这个倒金字塔 最下方 的格子。倒金字塔的高度是它所覆盖的行数。令 (r, c) 为金字塔的顶端且高度为 h ，那么金字塔区域内包含的任一格子 `(i, j)` 需满足 `r - h + 1 <= i <= r` 且 `c - (r - i) <= j <= c + (r - i)` 。
下图展示了部分符合定义和不符合定义的金字塔区域。黑色区域表示肥沃的格子。

![](https://assets.leetcode.com/uploads/2021/11/08/image.png)

给你一个下标从 0 开始且大小为 `m x n` 的二进制矩阵 `grid` ，它表示农场，请你返回 `grid` 中金字塔和倒金字塔的 总数目 。

 

示例 1：

  <img src="https://assets.leetcode.com/uploads/2021/10/23/eg11.png" style="zoom:50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/exa12.png" style="zoom:50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/exa13.png" style="zoom:50%;" />

```
输入：grid = [[0,1,1,0],[1,1,1,1]]
输出：2
解释：
2 个可能的金字塔区域分别如上图蓝色和红色区域所示。
这个网格图中没有倒金字塔区域。
所以金字塔区域总数为 2 + 0 = 2 。
```

示例 2：

  <img src="https://assets.leetcode.com/uploads/2021/10/23/eg21.png" style="zoom:50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/exa22.png" style="zoom:50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/exa23.png" style="zoom:50%;" />

```
输入：grid = [[1,1,1],[1,1,1]]
输出：2
```

解释：
金字塔区域如上图蓝色区域所示，倒金字塔如上图红色区域所示。
所以金字塔区域总数目为 1 + 1 = 2 。
示例 3：

<img src="https://assets.leetcode.com/uploads/2021/10/23/eg3.png" style="zoom:50%;" />

```
输入：grid = [[1,0,1],[0,0,0],[1,0,1]]
输出：0
解释：
网格图中没有任何金字塔或倒金字塔区域。
```

示例 4：

<img src="https://assets.leetcode.com/uploads/2021/10/23/eg41.png" style="zoom: 50%;" />   <img src="https://assets.leetcode.com/uploads/2021/10/23/eg42.png" style="zoom: 50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/eg43.png" style="zoom:50%;" /><img src="https://assets.leetcode.com/uploads/2021/10/23/eg44.png" style="zoom:50%;" />

```
输入：grid = [[1,1,1,1,0],[1,1,1,1,1],[1,1,1,1,1],[0,1,0,0,1]]
输出：13
解释：
有 7 个金字塔区域。上图第二和第三张图中展示了它们中的 3 个。
有 6 个倒金字塔区域。上图中最后一张图展示了它们中的 2 个。
所以金字塔区域总数目为 7 + 6 = 13.
```


提示：

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 1000`
- `1 <= m * n <= 10^5`
- `grid[i][j]` 要么是 0 ，要么是 1 。

# 思路

万万没想到这是个 DP 题，不过理应想到的，因为金字塔都是套娃的。

对于每个点，假设以它为顶点的最大金字塔高度为 `x` （不算顶点这一层），那么以它为顶点的金字塔数量就是 `x` 个，每个层一个嘛。看图还能发现，当这个点为 1 的时候，只要左下、正下、右下三个位置有小金字塔，他就有金字塔，并且深度 `x` 就是三个小金字塔最小深度再加一，递推关系就这么出来了。

倒金字塔就是反过来算一遍。

# 代码

```c++
class Solution {
public:
    int countPyramids(vector<vector<int>> &grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> f(m, vector<int>(n));
        int ans = 0;
        // 金字塔
        for (int i = m - 1; i >= 0; --i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == m - 1 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = min({f[i + 1][j - 1], f[i + 1][j], f[i + 1][j + 1]}) + 1;
                    ans += f[i][j];
                }
            }
        }
        // 倒金字塔
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (grid[i][j] == 0) {
                    f[i][j] = -1;
                } else if (i == 0 || j == 0 || j == n - 1) {
                    f[i][j] = 0;
                } else {
                    f[i][j] = min({f[i - 1][j - 1], f[i - 1][j], f[i - 1][j + 1]}) + 1;
                    ans += f[i][j];
                }
            }
        }

        return ans;
    }
};
```

