---
layout: post
title: "刷题记录-参加考试的最大学生数"
subtitle: ""
date: 2022-02-10
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Hard","DP","位运算","状态压缩"]
---

> https://leetcode-cn.com/problems/maximum-students-taking-exam/

# 题目

给你一个 `m * n` 的矩阵 `seats` 表示教室中的座位分布。如果座位是坏的（不可用），就用 `'#'` 表示；否则，用 `'.'` 表示。

学生可以看到左侧、右侧、左上、右上这四个方向上紧邻他的学生的答卷，但是看不到直接坐在他前面或者后面的学生的答卷。请你计算并返回该考场可以容纳的一起参加考试且无法作弊的最大学生人数。

学生必须坐在状况良好的座位上。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/09/image.png)

```
输入：seats = [["#",".","#","#",".","#"],
              [".","#","#","#","#","."],
              ["#",".","#","#",".","#"]]
输出：4
解释：教师可以让 4 个学生坐在可用的座位上，这样他们就无法在考试中作弊。 
```

示例 2：

```
输入：seats = [[".","#"],
              ["#","#"],
              ["#","."],
              ["#","#"],
              [".","#"]]
输出：3
解释：让所有学生坐在可用的座位上。
```

示例 3：

```
输入：seats = [["#",".",".",".","#"],
              [".","#",".","#","."],
              [".",".","#",".","."],
              [".","#",".","#","."],
              ["#",".",".",".","#"]]
输出：10
解释：让学生坐在第 1、3 和 5 列的可用座位上。
```


提示：

- `seats` 只包含字符 `'.'` 和 `'#'`
- `m == seats.length`
- `n == seats[i].length`
- `1 <= m <= 8`
- `1 <= n <= 8`

# 思路

位运算 + 动态规划 + 状态压缩，直接抄题解了……看代码注释吧。

# 代码

```c++
class Solution {
public:
    int maxStudents(vector <vector<char>> &seats) {
        int m = seats.size();
        int n = seats[0].size();
        vector <vector<int>> dp(m + 1, vector<int>(1 << n));  //初始化

        for (int row = 1; row <= m; row++) {
            for (int s = 0; s < (1 << n); s++) {//遍历 2^n 个状态
                bitset<8> bs(s);//记录对应状态的bit位
                bool ok = true;
                for (int j = 0; j < n; j++) {
                    if ((bs[j] && seats[row - 1][j] == '#') || (j < n - 1 && bs[j] && bs[j + 1])) {//不能坐在坏椅子上也不能在同一行相邻坐
                        ok = false;
                        break;
                    }
                }
                if (!ok) {
                    dp[row][s] = -1;//说明坐在坏椅子上或相邻坐了，该状态舍弃
                    continue;
                }
                for (int last = 0; last < (1 << n); last++) {//找到一种当前行的可行状态后，遍历上一行的所有状态
                    if (dp[row - 1][last] == -1)//上一行的状态被舍弃了，那就直接下一个状态
                        continue;
                    bitset<8> lbs(last);
                    bool flag = true;
                    for (int j = 0; j < n; j++) {
                        if (lbs[j] && ((j > 0 && bs[j - 1]) || (j < n - 1 && bs[j + 1]))) {//如果找到的这个上一行状态的j位置坐了人，
                            flag = false;                                    //下一行的j+1位置或j-1位置也坐了人，那么该状态不合法，舍弃
                            break;
                        }
                    }
                    if (flag) {//flag为真说明这个last状态的每个位置都合法
                        dp[row][s] = max(dp[row][s], dp[row - 1][last] + (int) bs.count());//转移方程
                    }
                }

            }
        }
        int res = 0;
        for (int i = 0; i < (1 << n); i++) {//在最后一行的所有状态中找出最大的
            if (dp[m][i] > res) {
                res = dp[m][i];
            }
        }
        return res;
    }
};
```

