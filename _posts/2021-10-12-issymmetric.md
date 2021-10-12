---
layout: post
title: "刷题记录-对称二叉树"
subtitle: ""
date: 2021-10-12
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二叉树"]
---

# 题目

给定一个二叉树，检查它是否是镜像对称的。

 

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

    		1
       / \
      2   2
     / \ / \
    3  4 4  3

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

    		1
       / \
      2   2
       \   \
       3    3

进阶：

- 你可以运用递归和迭代两种方法解决这个问题吗？

# 思路

一开始没想到，还在想中序遍历再判断数组对称。但是题目没有给数据范围，没法表示不存在的节点。

实际上直接用双指针就好了，一个往左走一个往右走。

如果用迭代代替递归，就引入队列，一次取出两个。

# 代码

```c++
class Solution {
public:
    bool helper(TreeNode *p, TreeNode *q) {
        if (!p && !q) return true;
        else if (!p || !q) return false;
        return p->val == q->val && helper(p->left, q->right) && helper(p->right, q->left);
    }

    bool isSymmetric(TreeNode *root) {
        return helper(root);
    }

    bool helper(TreeNode *root) {
        queue<TreeNode *> q;
        q.push(root);
        q.push(root);
        while (!q.empty()) {
            TreeNode *first = q.front();
            q.pop();
            if (q.empty()) return false;
            TreeNode *second = q.front();
            q.pop();
            if (!first && !second) continue;
            else if (!first || !second) return false;
            if (first->val != second->val) return false;
            q.push(first->left);
            q.push(second->right);
            q.push(first->right);
            q.push(second->left);
        }
        return true;
    }
};
```

