---
layout: post
title: "机器学习基石第2课：PLA"
author: Dervean
description: "机器学习基石第2课"
categories: [machine learning]
tags: [machine learning]
redirect_from:
  - /2018/01/29/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

## Perceptron Learning Algorithm

### perceptrons $\Leftrightarrow$ linear(binary) classifiers

感知器(perceptrons)就是一个线性分类器，用于解决二分类问题，例如回答是/不是。

perceptron 可以看成一个简单的hypothesis set（$\mathcal{H}$），还是使用信贷公司公司的例子：
* 用户的年龄、每月收入以及欠款额度等信息记作$x = (x_1,x_2,...,x_d)$，称作用户的**特征**(features)，如果每个特征都赋予一个权重分数w，计算总分数 $\sum_{i=1}^d w_ix_i$，给定一个threshold，
当$\sum_{i=1}^d w_ix_i > threshold$时接受发贷款；
当$\sum_{i=1}^d w_ix_i < thredshold$时拒绝发贷款。
* $y:\left\{+1,-1\right\}$，线性公式$h \in \mathcal{H}$：

$$
\begin{array}{rcl}
h(x)	&	=	&	sign((\sum_{i=1}^d w_ix_i)-threshold)      \\
		&	=	&	sign((\sum_{i=1}^d w_ix_i)+ (\underbrace{-threshold})\cdotp(\underbrace{+1})  \\
		&	=	&	sign(\sum_{i=0}^d w_ix_i)  \\
		&	=	&	sign(W^TX)
\end{array}
$$


![definition](/images/ML/definition-ML.png "definition")