---
layout: post
title: "刷题记录-Average Height of Buildings in Each Segment"
subtitle: ""
date: 2021-11-02
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","优先队列","建筑物"]
---

# 题目

A perfectly straight street is represented by a number line. The street has building(s) on it and is represented by a 2D integer array buildings, where `buildings[i] = [starti, endi, heighti]`. This means that there is a building with heighti in the half-closed segment [starti, endi).

You want to describe the heights of the buildings on the street with the minimum number of non-overlapping segments. The street can be represented by the 2D integer array street where s`treet[j] = [leftj, rightj, averagej]` describes a half-closed segment `[leftj, rightj)` of the road where the average heights of the buildings in the segment is averagej.

For example, if `buildings = [[1,5,2],[3,10,4]]`, the street could be represented by `street = [[1,3,2],[3,5,3],[5,10,4]]` because:

- From 1 to 3, there is only the first building with an average height of 2 / 1 = 2.
- From 3 to 5, both the first and the second building are there with an average height of (2+4) / 2 = 3.
- From 5 to 10, there is only the second building with an average height of 4 / 1 = 4.

Given buildings, return the 2D integer array street as described above (excluding any areas of the street where there are no buldings). You may return the array in any order.

The average of n elements is the sum of the n elements divided (integer division) by n.

A half-closed segment `[a, b)` is the section of the number line between points a and b including point a and not including point b. 

Example 1:

![](https://assets.leetcode.com/uploads/2021/09/21/image-20210921224001-2.png)

```
Input: buildings = [[1,4,2],[3,9,4]]
Output: [[1,3,2],[3,4,3],[4,9,4]]
Explanation:
From 1 to 3, there is only the first building with an average height of 2 / 1 = 2.
From 3 to 4, both the first and the second building are there with an average height of (2+4) / 2 = 3.
From 4 to 9, there is only the second building with an average height of 4 / 1 = 4.
```


Example 2:

```
Input: buildings = [[1,3,2],[2,5,3],[2,8,3]]
Output: [[1,3,2],[3,8,3]]
Explanation:
From 1 to 2, there is only the first building with an average height of 2 / 1 = 2.
From 2 to 3, all three buildings are there with an average height of (2+3+3) / 3 = 2.
From 3 to 5, both the second and the third building are there with an average height of (3+3) / 2 = 3.
From 5 to 8, there is only the last building with an average height of 3 / 1 = 3.
The average height from 1 to 3 is the same so we can group them into one segment.
The average height from 3 to 8 is the same so we can group them into one segment.
```

Example 3:

```
Input: buildings = [[1,2,1],[5,6,1]]
Output: [[1,2,1],[5,6,1]]
Explanation:
From 1 to 2, there is only the first building with an average height of 1 / 1 = 1.
From 2 to 5, there are no buildings, so it is not included in the output.
From 5 to 6, there is only the second building with an average height of 1 / 1 = 1.
We cannot group the segments together because an empty space with no buildings seperates the segments.
```


Constraints:

- `1 <= buildings.length <= 10^5`
- `buildings[i].length == 3`
- `0 <= starti < endi <= 10^8`
- `1 <= heighti <= 10^5`

# 思路

遇到类似问题，需要把进入和离开建筑物的点拆出来，按位置坐标排序。同时记录好当前的建筑物数量、高度，来判断当前状态和是否需要更新答案：

- 在建筑物内，进入或离开建筑物后仍然在建筑物内：检查新的平均高度，如果和之前不一样就更新答案
- 在建筑物内，离开所有建筑物：更新答案，重置高度和数量
- 不在建筑物内，进入建筑物：更新高度和数量

# 代码

```c++
class Solution {
public:
    using pos_type_height = vector<int>;

    vector<vector<int>> averageHeightOfBuildings(vector<vector<int>> &buildings) {
        priority_queue<pos_type_height, vector<pos_type_height>, greater<pos_type_height>> pq;
        for (auto &build: buildings) {
            pq.push({build[0], 1, build[2]});
            pq.push({build[1], -1, -build[2]});
        }
        vector<vector<int>> res;
        int height = 0, cnt = 0, left = -1;
        while (!pq.empty()) {
            int h = height, c = cnt;
            pos_type_height curr = pq.top();
            pq.pop();
            h += curr[2];
            c += curr[1];
            // with same position
            while (!pq.empty() && pq.top()[0] == curr[0]) {
                pos_type_height temp = pq.top();
                pq.pop();
                // sum up height and cnt at this position
                h += temp[2];
                c += temp[1];
            }
            // cnt: in building before this position
            // c: in building at this position
            if (cnt && c) {
                // still in building, maybe leaving or entering
                // average height changed
                if (height / cnt != h / c) {
                    res.push_back({left, curr[0], height / cnt});
                    left = curr[0];
                }
                height = h;
                cnt = c;
            } else if (cnt) {
                // leaving building
                res.push_back({left, curr[0], height / cnt});
                height = 0;
                cnt = 0;
                left = -1;
            } else if (c) {
                // entering building
                height = h;
                cnt = c;
                left = curr[0];
            }
        }
        return res;
    }
};
```

