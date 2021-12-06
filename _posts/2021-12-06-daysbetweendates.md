---
layout: post
title: "刷题记录-日期之间隔几天"
subtitle: ""
date: 2021-12-06
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","Easy"]
---

> https://leetcode-cn.com/problems/number-of-days-between-two-dates/

# 题目

请你编写一个程序来计算两个日期之间隔了多少天。

日期以字符串形式给出，格式为 YYYY-MM-DD，如示例所示。

 

示例 1：

```
输入：date1 = "2019-06-29", date2 = "2019-06-30"
输出：1
```

示例 2：

```
输入：date1 = "2020-01-15", date2 = "2019-12-31"
输出：15
```


提示：

- 给定的日期是 1971 年到 2100 年之间的有效日期。

# 思路

转成到一个固定日期的天数差，然后做差取绝对值即可。

注意闰年的判断条件：

- 能被 400 整除
- 能被 4 整除，但不能被 100 整除

# 代码

```c++
class Solution {
public:
    int monthdays[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

    bool isLeap(int year) {
        return ((((year) % 4) == 0 && ((year) % 100) != 0) || ((year) % 400) == 0);
    }

    int daysBetweenEpoch(string date1) {
        int years = stoi(date1.substr(0, 4));
        int months = stoi(date1.substr(5, 2));
        int days = stoi(date1.substr(8, 2));
        int result = 0;
        for (int y = 1970; y < years; ++y) {
            if (isLeap(y)) result += 366;
            else result += 365;
        }
        for (int m = 1; m < months; ++m) {
            if (m == 2 && isLeap(years)) result += 29;
            else result += monthdays[m];
        }
        for (int d = 1; d < days; ++d) {
            result += 1;
        }
        return result;

    }

    int daysBetweenDates(string date1, string date2) {
        return abs(daysBetweenEpoch(date1) - daysBetweenEpoch(date2));
    }
};
```

