---
layout: post
title: "机器学习基石第11课：Linear Models for Classification"
author: Dervean
description: "机器学习基石第11课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/26/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Linear Models for Binary Classification

在前面的课程中介绍了三种线性模型:线性分类、线性回归和逻辑回归。其中线性回归问题可以得到 close-form solution，逻辑回归问题可以使用梯度下降解决，但计算线性分类问题的 $E_{in}(w)$ 是 NP-hard 的。

那能否**使用线性回归或者逻辑回归来解决线性分类问题**呢？如果可以，理论依据是什么呢？

下面来分析三种模型的 Error Measure:

- linear classification: 

  $$
  \begin{array}{rcl}
  err_{0/1}(s,y) & = & [\![ sign(s) \neq y ]\!] \\
  				 & = & [\![ sign(ys) \neq 1 ]\!]
  \end{array}
  $$

- linear regression: 

  $$
  \begin{array}{rcl}
  err_{SQR}(s,y) & = & (s - y)^2 \\
  				 & = & (ys - 1)^2
  \end{array}
  $$

- linear classification: 

  $$
  err_{CE}(s,y) = \ln(1 + \exp(-ys))
  $$

以上 $y \in \\{-1,+1\\}$，$(ys)$ 可以用来看作一个整体来判断分类的正确性。

![linear-models-for-classification-1](/images/machine-learning-foundations/linear-models-for-classification-1.png "Error Bound")

稍微改变(Scale) linear classification 的 Error Measure:

$$
err_{SCE}(s,y) = \log_2(1 + \exp(-ys))
$$

![linear-models-for-classification-2](/images/machine-learning-foundations/linear-models-for-classification-2.png "Error Bound")

可以看出 $err_{SQR}$ 和 $err_{CE}$ 都是 $err_{0/1}$ 的 upper bound.

$$
\text{small } E_{in}^{CE}(w) \Longrightarrow \text{small } E_{out}^{0/1}(w)
$$

linear regression sometimes used to set $w_0$ for PLA/pocket/logistic regression.

# Stochastic Gradient Descent

上节课说逻辑回归:

$$
w_{t+1} \leftarrow w_t + \eta \underbrace{\frac{1}{N} \sum_{n=1}^N (\theta (-y_nw^Tx_n))(y_nx_n)}_{-\triangledown E_{in}(w)}
$$

从上述公式看出，每做一步更新就需要遍历一次所有数据点（即花销为 $O(N)$），然而在 Pocket Algorithm 中每次更新只需要 $O(1)$ 的花销，我们能不能减小逻辑回归更新花销呢？

事实上可以将每一轮更新的复杂度降为 $O(1)$，这时候就需要使用**随机梯度下降**。

重新研究上面的公式可以发现: $\frac{1}{N} \sum_{n=1}^N$，即计算所有的数据之后再平均。

所以可以**每次随机选取一个点计算梯度**用来代替总体平均，当数据点不是一次性提供或者 online learning 的时候就很有用，但这种方法**没办法知道啥时候算法能停止**，在使用的时候假设在经过**很多很多**步之后算法会选到最优点:

$$
w_{t+1} \leftarrow w_t + \eta \underbrace{(\theta (-y_nw_t^Tx_n))(y_nx_n)}_{-\triangledown err(w_t,x_n,y_n)}
$$
































