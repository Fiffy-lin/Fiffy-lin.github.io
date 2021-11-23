---
layout: post
title: "刷题记录-二叉树的最近公共祖先"
subtitle: ""
date: 2021-11-25
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","二叉树"]
---

# 题目

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

 

示例 1：

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

示例 2：

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```


示例 3：

```
输入：root = [1,2], p = 1, q = 2
输出：1
```


提示：

- 树中节点数目在范围 `[2, 10^5]` 内。
- `-10^9 <= Node.val <= 10^9`
- 所有 `Node.val` 互不相同 。
- `p != q`
- `p` 和 `q` 均存在于给定的二叉树中。

# 思路

由示例二可以得知，如果当前节点是找的俩中的一个，那么直接返回就行了。因为我们是从根开始遍历的，先碰到的一定是更高的。

如果这个树里面只存在一个节点，那么就返回这个节点就行了。如果这个树左右子树的节点都有返回值，那么当前树根就是最大深度公共祖先。

如果一边有结果，一边没结果，说明要么都在一边里，要么只有一个，那么就返回这个有值的就行了。

# 代码

```c++
class Solution {
public:
    TreeNode *lowestCommonAncestor(TreeNode *root, TreeNode *p, TreeNode *q) {
        if (!root) return nullptr;
        if (root == p || root == q) return root;
        TreeNode *left_res = lowestCommonAncestor(root->left, p, q);
        TreeNode *right_res = lowestCommonAncestor(root->right, p, q);
        if (left_res && right_res) return root;
        else return left_res ? left_res : right_res;
    }
};
```

