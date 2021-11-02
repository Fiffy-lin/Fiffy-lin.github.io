---
layout: post
title: "刷题记录-会议室 II"
subtitle: ""
date: 2021-11-02
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","堆"]
---

# 题目

给你一个会议时间安排的数组 `intervals` ，每个会议时间都会包括开始和结束的时间 `intervals[i] = [starti, endi]` ，为避免会议冲突，同时要考虑充分利用会议室资源，请你计算至少需要多少间会议室，才能满足这些会议安排。

示例 1：

```
输入：intervals = [[0,30],[5,10],[15,20]]
输出：2
```

示例 2：

```
输入：intervals = [[7,10],[2,4]]
输出：1
```


提示：

- `1 <= intervals.length <= 10^4`
- `0 <= starti < endi <= 10^6`

# 思路

可以按时间段题目的标准做法来，把开始时间和结束时间以及状态放入优先队列，遍历整个队列，同时统计会议室数目的最大值。

更简单的做法是把结束时间放入优先队列。队首元素是目前最早结束会议的时间，和当前会议开始时间比较，如果当前开始时间更早，就入队，否则直接修改队首。

# 代码

```c++
class Solution {
public:
    int minMeetingRooms(vector<vector<int>> &intervals) {
        sort(intervals.begin(), intervals.end());
        priority_queue<int, vector<int>, greater<int>> end_time;
        for (auto start_end: intervals) {
            if (!end_time.empty() && end_time.top() <= start_end[0]) end_time.pop();
            end_time.push(start_end[1]);
        }
        return end_time.size();
    }

    int minMeetingRooms2(vector<vector<int>> &intervals) {
        using time_type = pair<int, int>;
        priority_queue<time_type, vector<time_type>, greater<time_type>> time_type_queue;
        for (auto start_end: intervals) {
            time_type_queue.emplace(start_end[0], 1);
            time_type_queue.emplace(start_end[1], -1);
        }
        int cur = 0, maxi = 0;
        while (!time_type_queue.empty()) {
            time_type curr = time_type_queue.top();
            time_type_queue.pop();
            cur += curr.second;
            while (!time_type_queue.empty() && time_type_queue.top().first == curr.first) {
                cur += time_type_queue.top().second;
                time_type_queue.pop();
            }
            maxi = max(maxi, cur);
        }
        return maxi;
    }
};
```

