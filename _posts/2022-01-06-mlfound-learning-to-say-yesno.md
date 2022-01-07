---
layout: post
title: "机器学习基石-2-Learning To Say Yes/No"
subtitle: ""
date: 2022-01-06
author: "Fiffy"
header-img: "img/post-bg-2015.jpg"
tags: ["Course","机器学习基石"]
---

# Perceptron Hypothesis

Multiply each feature with its coresponding weight, sum those value to, and compare to threshold. This is called perceptron hypothesis.

Learned function: `h(x) = sign(sum(Wi*Xi) - threshold)`

` - threshold ` can be written as ` + (- threshold) * 1` , `(- threshold)` can be seen as `W0`, 1 can be seen as `X0`.

So function can be written as `h(x) = sign(sum(Wi*Xi)) = sign(WTX)`

# Cyclic PLA

For t  = 0,1....

1. Find a mistake of Wt called (xt,yt)
2. Correct it. Wt+1 = Wt + ytxt
3. Till no more mistakes.

Prove why PLA will halt:

![image-20220110205232818](/img/in-post/post-2022-01-06/image-20220110205232818.png)

# Linear Separability

