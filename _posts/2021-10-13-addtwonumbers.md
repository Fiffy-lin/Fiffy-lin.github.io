---
layout: post
title: "刷题记录-两数相加 II"
subtitle: ""
date: 2021-10-13
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","栈","链表"]
---

# 题目

给你两个 非空 链表来代表两个非负整数。数字最高位位于链表开始位置。它们的每个节点只存储一位数字。将这两数相加会返回一个新的链表。

你可以假设除了数字 0 之外，这两个数字都不会以零开头。

示例1：

![](https://pic.leetcode-cn.com/1626420025-fZfzMX-image.png)

```
输入：l1 = [7,2,4,3], l2 = [5,6,4]
输出：[7,8,0,7]
```

示例2：

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[8,0,7]
```

示例3：

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

提示：

- 链表的长度范围为 [1, 100]
- `0 <= node.val <= 9`
- 输入数据保证链表代表的数字无前导 0


进阶：如果输入链表不能修改该如何处理？换句话说，不能对列表中的节点进行翻转。

# 思路

能反转就反转相加，不能反转就用栈。

逆序的题目首先应该想到栈，对吧？

# 代码

```c++
class Solution {
public:
    ListNode *reverseList(ListNode *head) {
        if (head == nullptr || head->next == nullptr) return head;
        ListNode *p = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
        return p;
    }

    void headInsert(ListNode **root, int val) {
        if (!(*root)) *root = new ListNode(val);
        else *root = new ListNode(val, *root);
    }

    ListNode *addTwoNumbers1(ListNode *l1, ListNode *l2) {
        int len_l1 = 0, len_l2 = 0;
        for (ListNode *curr = l1; curr; curr = curr->next) len_l1++;
        for (ListNode *curr = l2; curr; curr = curr->next) len_l2++;
        ListNode **shorter = len_l1 < len_l2 ? &l1 : &l2;
        for (int i = abs(len_l1 - len_l2); i > 0; --i)
            headInsert(shorter, 0);
        ListNode *rev_l1 = reverseList(l1);
        ListNode *rev_l2 = reverseList(l2);
        ListNode *ans = nullptr;
        int carry = 0;
        while (rev_l1 || carry) {
            if (!rev_l1) {
                headInsert(&ans, carry);
                break;
            }
            int val = rev_l1->val + rev_l2->val + carry;
            if (val >= 10) {
                val -= 10;
                carry = 1;
            } else carry = 0;
            headInsert(&ans, val);
            rev_l1 = rev_l1->next;
            rev_l2 = rev_l2->next;
        }
        return ans;
    }

    ListNode *addTwoNumbers(ListNode *l1, ListNode *l2) {
        stack<int> s1, s2;
        int carry = 0;
        for (ListNode *curr = l1; curr; curr = curr->next) s1.push(curr->val);
        for (ListNode *curr = l2; curr; curr = curr->next) s2.push(curr->val);
        ListNode *ans = nullptr;
        while (!s1.empty() || !s2.empty() || carry) {
            int val1 = s1.empty() ? 0 : s1.top();
            int val2 = s2.empty() ? 0 : s2.top();
            if (!s1.empty()) s1.pop();
            if (!s2.empty()) s2.pop();
            int sum = val1 + val2 + carry;
            sum = (sum) > 9 ? (carry = 1, sum - 10) : (carry = 0, sum);
            headInsert(&ans, sum);
        }
        return ans;
    }
};
```

