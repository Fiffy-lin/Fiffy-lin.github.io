---
layout: post
title: "刷题记录-二叉搜索树节点最小距离"
subtitle: ""
date: 2021-10-28
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二叉树"]
---

# 题目

给你一个二叉搜索树的根节点 `root` ，返回 树中任意两不同节点值之间的最小差值 。

注意：本题与 530：https://leetcode-cn.com/problems/minimum-absolute-difference-in-bst/ 相同

示例 1：

![](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

```
输入：root = [4,2,6,1,3]
输出：1
```

示例 2：

![](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

```
输入：root = [1,0,48,null,null,12,49]
输出：1
```


提示：

- 树中节点数目在范围 [2, 100] 内
- `0 <= Node.val <= 10^5`
- 差值是一个正数，其数值等于两值之差的绝对值

# 思路

BST 的中序遍历是从小到大的顺序，因此用一个全局变量保存上一个数就可以了。

# 代码

```c++
class Solution {
public:
    int pre = -1, min_diff = INT_MAX;

    void pre_order(TreeNode *root) {
        if (!root) return;
        pre_order(root->left);
        if (pre != -1) min_diff = min(min_diff, abs(root->val - pre));
        pre = root->val;
        pre_order(root->right);
    }

    int minDiffInBST(TreeNode *root) {
        pre_order(root);
        return min_diff;
    }
};
```

