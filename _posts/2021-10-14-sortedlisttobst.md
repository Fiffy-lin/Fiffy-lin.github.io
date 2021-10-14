---
layout: post
title: "刷题记录-有序链表转换二叉搜索树"
subtitle: ""
date: 2021-10-14
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","链表","二叉树"]
---

# 题目

给定一个单链表，其中的元素按升序排序，将其转换为高度平衡的二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1。

示例:

给定的有序链表： `[-10, -3, 0, 5, 9]`,

一个可能的答案是：`[0, -3, 9, -10, null, 5]`, 它可以表示下面这个高度平衡二叉搜索树：

         0
        / \
      -3   9
      /   /
    -10  5
# 思路

层次建树，中序遍历填入值。

题解思路：分治+中序遍历。分治保证了建树的平衡性。中序遍历的时候，head 通过传引用可以实时更改值，我没有想到这一点。

# 代码

```c++
class Solution {
public:
    ListNode *curr_list;

    void helper(TreeNode *curr_tree) {
        if (!curr_tree) return;
        helper(curr_tree->left);
        curr_tree->val = curr_list->val;
        curr_list = curr_list->next;
        helper(curr_tree->right);
    }

    TreeNode *sortedListToBST(ListNode *head) {
        if (!head) return nullptr;
        int len = 0;
        for (ListNode *curr = head; curr; curr = curr->next) len++;
        queue<TreeNode *> q;
        TreeNode *root = new TreeNode;
        len--;
        q.push(root);
        while (len) {
            TreeNode *front = q.front();
            q.pop();
            front->left = new TreeNode();
            len--;
            q.push(front->left);
            if (len) {
                front->right = new TreeNode();
                len--;
                q.push(front->right);
            }
        }
        curr_list = head;
        helper(root);
        return root;
    }
};
```

```c++
class Solution {
public:
    int getLength(ListNode* head) {
        int ret = 0;
        for (; head != nullptr; ++ret, head = head->next);
        return ret;
    }

    TreeNode* buildTree(ListNode*& head, int left, int right) {
        if (left > right) {
            return nullptr;
        }
        int mid = (left + right + 1) / 2;
        TreeNode* root = new TreeNode();
        root->left = buildTree(head, left, mid - 1);
        root->val = head->val;
        head = head->next;
        root->right = buildTree(head, mid + 1, right);
        return root;
    }

    TreeNode* sortedListToBST(ListNode* head) {
        int length = getLength(head);
        return buildTree(head, 0, length - 1);
    }
};
```

