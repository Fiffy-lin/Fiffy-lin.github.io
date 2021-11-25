---
layout: post
title: "刷题记录-最大层内元素和"
subtitle: ""
date: 2021-11-26
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Leetcode","BFS","二叉树"]
---

# 题目

给你一个二叉树的根节点 `root`。设根节点位于二叉树的第 1 层，而根节点的子节点位于第 2 层，依此类推。

请你找出层内元素之和 最大 的那几层（可能只有一层）的层号，并返回其中 最小 的那个。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/08/17/capture.jpeg)

```
输入：root = [1,7,0,7,-8,null,null]
输出：2
解释：
第 1 层各元素之和为 1，
第 2 层各元素之和为 7 + 0 = 7，
第 3 层各元素之和为 7 + -8 = -1，
所以我们返回第 2 层的层号，它的层内元素之和最大。
```

示例 2：

```
输入：root = [989,null,10250,98693,-89388,null,null,null,-32127]
输出：2
```


提示：

- 树中的节点数介于 `1` 和 `10^4` 之间
- `-10^5 <= node.val <= 10^5`

# 思路

BFS/层序遍历秒了。

# 代码

```c++
class Solution {
public:
    int maxLevelSum(TreeNode *root) {
        queue<TreeNode *> q;
        q.push(root);
        int level = 1, max_sum = INT_MIN, max_lvl = 1;
        while (!q.empty()) {
            int n = q.size(), sum = 0;
            for (int i = 0; i < n; ++i) {
                TreeNode *front = q.front();
                q.pop();
                sum += front->val;
                if (front->left) q.push(front->left);
                if (front->right) q.push(front->right);
            }
            if (sum > max_sum) {
                max_sum = sum;
                max_lvl = level;
            }
            level++;
        }
        return max_lvl;
    }
};
```

