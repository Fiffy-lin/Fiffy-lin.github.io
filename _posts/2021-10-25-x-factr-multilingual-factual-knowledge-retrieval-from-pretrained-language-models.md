---
layout: post
title: "X FACTR: Multilingual Factual Knowledge Retrieval from Pretrained Language Models"
subtitle: ""
date: 2021-10-25
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Paper"]
---

# Background

最近在研究 prompt 在中文文本分类上的应用。不同于英文，中文的预训练语言模型往往都是字粒度的，大多数的分类任务类别名都由两字或以上组成，这就意味着 prompt-based model 的 answer 生成是一个多 token 解码过程。这篇文章里有关于多 token 解码的部分，拿来看一看。

# Paper

文章提出的方法把多 token 解码划分成了两个部分，initial 和 refinement，并提出了三种解码方式：

1. Independent, 即多个 token 独立解码，同时预测。我们的 prompt 原型也用了这个方法，会出现两个字分属不同的类别名称的问题，好处是简单。
2. Order-based, 按顺序解码。可以由左到右一次生成一个 token 并填入。
3. Confidence-based, 也是按顺序解码，但不是按左右顺序，而是按 token 的 confidence 从低到高一个一个重新预测。

这篇文章的任务不仅是多 token，也是不定长 token 任务。处理 token 长度的方法就是从 1 循环到 M，每个长度进行一次解码，并选择最高的 confidence 对应的解码后句子作为输出。简单的累加 confidence 会使模型偏向于预测短句子，因此作者还加上了 length normalization, 用 confidence/sum of masks 作为实际的 confidence: 

![Length Normalization](/img/in-post/post-2021-10-25/length-normalization.png)

同时，由于使用的是双向语言模型，每次生成一个 token 时，已经生成的 token 的 confidence 也会受到影响，因此生成一个 token 后要重新计算所有预测过的 token 的 confidence.

作者还引入了类似 beam search 的方法，每个 iteration 中都基于 confidence 保留前 B 个句子。这也使总的预测复杂度达到了 `O(M^2BT), where M is the maximal number of mask tokens, and T is the maximal number of iteration.`

总的预测逻辑如下：

![Algorithm1](/img/in-post/post-2021-10-25/algo.png)

# Thoughts

预测逻辑着实复杂，可以一试，但是实际上线还是要看场景。如果只是 batch 预测那没问题，但是实时计算估计速度会有点跟不上。
