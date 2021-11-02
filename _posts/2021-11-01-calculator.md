---
layout: post
title: "刷题记录-基本计算器 III"
subtitle: ""
date: 2021-11-01
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","栈"]
---

# 题目

实现一个基本的计算器来计算简单的表达式字符串。

表达式字符串只包含非负整数，算符 `+、-、*、/ `，左括号 `(` 和右括号 `)` 。整数除法需要 向下截断 。

你可以假定给定的表达式总是有效的。所有的中间结果的范围为 `[-2^31, 2^31 - 1]` 。

示例 1：

```
输入：s = "1+1"
输出：2
```

示例 2：

```
输入：s = "6-4/2"
输出：4
```

示例 3：

```
输入：s = "2*(5+5*2)/3+(6/2+8)"
输出：21
```

示例 4：

```
输入：s = "(2+6*3+5-(3*14/7+2)*5)+3"
输出：-12
```

示例 5：

```
输入：s = "0"
输出：0
```


提示：

- `1 <= s <= 10^4`
- `s` 由整数、`'+'、'-'、'*'、'/'、'(' 和 ')'` 组成
- `s` 是一个 有效的 表达式

# 思路

运算符栈 + 数栈，按照运算符优先级计算。

# 代码

```c++
class Solution {
public:
    stack<int> digits;
    stack<char> ops;
    unordered_map<char, int> rank;

    void calc() {
        int val2 = digits.top();
        digits.pop();
        int val1 = digits.top();
        digits.pop();
        char op = ops.top();
        ops.pop();
        switch (op) {
            case '+':
                digits.emplace(val1 + val2);
                break;
            case '-':
                digits.emplace(val1 - val2);
                break;
            case '*':
                digits.emplace(val1 * val2);
                break;
            case '/':
                digits.emplace(val1 / val2);
                break;
        }
    }

    int calculate(string s) {
        rank['+'] = 0;
        rank['-'] = 0;
        rank['*'] = 1;
        rank['/'] = 1;
        rank['('] = -1;
        for (int i = 0; i < s.length(); ++i) {
            if (s[i] >= '0' && s[i] <= '9') {
                int val = s[i] - '0';
                for (i++; i < s.length() && s[i] >= '0' && s[i] <= '9'; i++) val = val * 10 + s[i] - '0';
                i--;
                digits.emplace(val);
            } else if (s[i] == '(') ops.emplace(s[i]);
            else if (s[i] == ')') {
                while (ops.top() != '(') calc();
                ops.pop();
            } else {
                while (!ops.empty() && rank[ops.top()] >= rank[s[i]]) calc();
                ops.push(s[i]);
            }
        }
        while (!ops.empty()) calc();
        return digits.top();
    }
};
```

