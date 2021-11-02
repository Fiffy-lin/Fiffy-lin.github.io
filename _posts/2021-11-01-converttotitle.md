---
layout: post
title: "刷题记录-Excel表列名称"
subtitle: ""
date: 2021-11-01
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","数学"]
---

# 题目

给你一个整数 columnNumber ，返回它在 Excel 表中相对应的列名称。

例如：

```
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```


示例 1：

```
输入：columnNumber = 1
输出："A"
```

示例 2：

```
输入：columnNumber = 28
输出："AB"
```

示例 3：

```
输入：columnNumber = 701
输出："ZY"
```

示例 4：

```
输入：columnNumber = 2147483647
输出："FXSHRXW"
```


提示：

- `1 <= columnNumber <= 2^31 - 1`

# 思路

本质上是十进制转 26 进制的问题，但是麻烦就麻烦在他的 26 进制是 1-26，而不是 0-25。

所以考虑在处理每一位的时候，先 -1，让范围从 1-26 变成 0-25，就可以正常取模。

证明过程见[题解](https://leetcode-cn.com/problems/excel-sheet-column-title/solution/excelbiao-lie-ming-cheng-by-leetcode-sol-hgj4/)。

# 代码

```c++
class Solution {
public:
    string convertToTitle(int columnNumber) {
        string ans;
        while(columnNumber) {
            --columnNumber;
            int re = columnNumber % 26;
            ans.push_back('A' + re);
            columnNumber /= 26;
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

