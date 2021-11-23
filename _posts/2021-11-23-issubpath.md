---
layout: post
title: "刷题记录-二叉树中的列表"
subtitle: ""
date: 2021-11-23
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二叉树","链表"]
---

# 题目

给你一棵以 `root` 为根的二叉树和一个 `head` 为第一个节点的链表。

如果在二叉树中，存在一条一直向下的路径，且每个点的数值恰好一一对应以 `head` 为首的链表中每个节点的值，那么请你返回 `True` ，否则返回 `False` 。

一直向下的路径的意思是：从树中某个节点开始，一直连续向下的路径。



示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_1_1720.png)

```
输入：head = [4,2,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
解释：树中蓝色的节点构成了与链表对应的子路径。
```

示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/02/29/sample_2_1720.png)

```
输入：head = [1,4,2,6], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：true
```

示例 3：

```
输入：head = [1,4,2,6,8], root = [1,4,4,null,2,2,null,1,null,6,8,null,null,null,null,1,3]
输出：false
解释：二叉树中不存在一一对应链表的路径。
```


提示：

- 二叉树和链表中的每个节点的值都满足 `1 <= node.val <= 100` 。
- 链表包含的节点数目在 `1` 到 `100` 之间。
- 二叉树包含的节点数目在 `1` 到 `2500` 之间。

# 思路

和判断子树一毛一样。考虑这几种情况：

1. 以根为起点进入判断：
   1. 如果到达某个节点，链表匹配完了，说明找到了对应路径
   2. 树匹配完了，说明走错了
   3. 当前值不相等，走错了
   4. 当前值相等，继续往左下右下走
2. 根判断完了，如果没成功，继续拿左下右下当根判断

第一次做的误区在于想在根为起点、值不相等的时候重新开始匹配。

# 代码

```c++
class Solution {
public:
    bool helper(ListNode *head, TreeNode *root) {
        if (!head) return true;
        if (!root) return false;
        if (head->val != root->val) return false;
        return helper(head->next, root->left) || helper(head->next, root->right);
    }

    bool isSubPath(ListNode *head, TreeNode *root) {
        if (!root) return false;
        return helper(head, root) || isSubPath(head, root->left) || isSubPath(head, root->right);
    }
};
```

