---
layout: post
title: "刷题记录-翻转二叉树"
subtitle: ""
date: 2021-10-15
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二叉树"]
---

# 题目

翻转一棵二叉树。

示例：

输入：

     		  4
        /   \
       2     7
      / \   / \
     1   3 6   9

输出：

     		 4
       /   \
      7     2
     / \   / \
    9   6 3   1


备注:

> 这个问题是受到 Max Howell 的 原问题 启发的 ： 

>  谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

# 思路

递归翻转。先翻转下面的左右子树再交换。

# 代码

```
class Solution {
public:
    TreeNode *invertTree(TreeNode *root) {
        if (!root) return nullptr;
        TreeNode *left = invertTree(root->left);
        TreeNode *right = invertTree(root->right);
        root->left = right;
        root->right = left;
        return root;
    }
};
```
