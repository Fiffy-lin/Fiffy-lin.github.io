---
layout: post
title: "刷题记录-基本计算器"
subtitle: ""
date: 2021-11-30
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","栈"]
---

> https://leetcode-cn.com/problems/basic-calculator/

# 题目

给你一个字符串表达式 `s` ，请你实现一个基本计算器来计算并返回它的值。

 

示例 1：

```
输入：s = "1 + 1"
输出：2
```

示例 2：

```
输入：s = " 2-1 + 2 "
输出：3
```

示例 3：

```
输入：s = "(1+(4+5+2)-3)+(6+8)"
输出：23
```


提示：

- `1 <= s.length <= 3 * 10^5`
- `s` 由数字、'+'、'-'、'('、')'、和 ' ' 组成
- `s` 表示一个有效的表达式

# 思路

这题多了几个包括负号的测试用例。负号就两种情况，一种是开头，可以通过往栈里先塞进去一个 0 来解决。一种是括号包裹负数，可以在括号和负号中间塞一个 0 变成双目减号。

计算思路还是和之前的一样。

# 代码

```c++
class Solution {
public:
    stack<char> op;
    stack<long long> val;
    unordered_map<char, int> priority;

    void calc() {
        long long val2 = val.top();
        val.pop();
        char oper = op.top();
        op.pop();
        long long val1 = val.top();
        val.pop();
        switch (oper) {
            case '+':
                val.push(val1 + val2);
                break;
            case '-':
                val.push(val1 - val2);
                break;
            case '*':
                val.push(val1 * val2);
                break;
            case '/':
                val.push(val1 / val2);
                break;
        }
    }

    int calculate(string s) {
        s.erase(remove(s.begin(), s.end(), ' '), s.end());
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '-' &&  (i - 1 >= 0 && s[i - 1] == '(')) s.insert(i, 1, '0');
        }
        val.push(0);
        priority['+'] = 0;
        priority['-'] = 0;
        priority['*'] = 1;
        priority['/'] = 1;
        for (int i = 0; i < s.length(); ++i) {
            char c = s[i];
            if (c == ' ') continue;
            if (isdigit(c)) {
                long long value = c - '0';
                while (i + 1 < s.length() && isdigit(s[i + 1])) {
                    i++;
                    value = value * 10 + s[i] - '0';
                }
                val.push(value);
            } else {
                if (c == '(') op.push(c);
                else if (c == ')') {
                    while (op.top() != '(') calc();
                    op.pop();
                } else {
                    while (!op.empty() && op.top() != '(' && priority[op.top()] >= priority[c]) calc();
                    op.push(c);
                }
            }
        }
        while (!op.empty()) calc();
        return int(val.top());
    }
};
```

