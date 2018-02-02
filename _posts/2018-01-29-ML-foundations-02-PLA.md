---
layout: post
title: "机器学习基石第2课：PLA"
author: Dervean
description: "机器学习基石第2课"
categories: [ML]
tags: [ML]
redirect_from:
  - /2018/01/29/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Perceptrons $\Leftrightarrow$ Linear(binary) classifiers

感知器(perceptrons)是一个线性分类器，用于解决二分类问题，例如回答是/不是。

perceptron 可以看成一个简单的hypothesis set（$H$），还是使用信贷公司的例子：
* 用户的年龄、每月收入以及欠款额度等信息记作$x = (x_1,x_2,...,x_d)$，称作用户的**特征**($features$)，如果每个特征都赋予一个权重分数w，计算总分数 $\sum_{i=1}^d w_ix_i$。
* 给定一个$threshold$，当$\sum_{i=1}^d w_ix_i > threshold$时接受发贷款；当$\sum_{i=1}^d w_ix_i < thredshold$时拒绝发贷款。
* $y:\left\\{+1,-1\right\\}$，线性公式$h \in H$：

$$
\begin{array}{rcl}
h(x)	&	=	&	sign((\sum_{i=1}^d w_ix_i)-threshold)      \\
		&	=	&	sign((\sum_{i=1}^d w_ix_i)+ (-threshold)\cdotp(+1))  \\
		&	=	&	sign(\sum_{i=0}^d w_ix_i)  \\
		&	=	&	sign(W^TX)
\end{array}
$$

* 当$h(x)>0$则$y=1$；当$h(x)<0$则$y=-1$。

![perceptrons](/images/ML/perceptrons.png "perceptrons")

我们已经知道所有的$H={perceptrons}$，如何从$H$中选择一个$g=hypothesis$呢？

* want: $g \approx f$，要求$g$在测试集$D$上大体满足甚至完全满足。 
* difficult: $H$ is of **infinite** size.
* idea: start from some $g_0$, 然后不断根据测试集$D$修改$g$. 

# Perceptron Learning Algorithm

![PLA](/images/ML/PLA.png "PLA")

如上图所示，PLA算法选择以不断“改错”来求得最终解，里面应用到一个有趣的向量计算的方法：对于两个向量$\vec{a}$和$\vec{b}$，

* $\vec{a}+\vec{b}$与$\vec{b}$的夹角小于$\vec{a}$与$\vec{b}$的夹角；
* $\vec{a}-\vec{b}$与$\vec{b}$的夹角大于$\vec{a}$与$\vec{b}$的夹角。

当实际$y=+1$而判断为$-1$时，则使用$\vec{w}+\vec{x}$代替$\vec{w}$;

当实际$y=-1$而判断为$+1$时，则使用$\vec{w}-\vec{x}$代替$\vec{w}$.

直到测试集中所有的点都符合分类要求，则将$W$作为$g$返回.

![PLA-implementation](/images/ML/PLA-implementation.png "PLA-implementation")

## 数据线性可分和PLA的算法有穷性

PLA算法要求最终能够找到一个直线（或者超平面）能够将所有数据点一分为二，但如果找不到怎么办呢？那是否意味着PLA算法有可能永远不会停止？

数据（$D$）的线性可分（linear separable）：

![PLA-linear-seperable](/images/ML/PLA-linear-seperable.png "PLA-linear-seperable")

可以证明：linear separable $D$ $\Leftrightarrow$ exists perfect $w_f$ such that $y_n=sign(w_f^Tx_n)$.

(1) $w_t$与$w_f$的夹角越来越小，即越来越接近：

![PLA-proof-1](/images/ML/PLA-proof-1.png "PLA-proof-1")

(2) $w_t$不会增长得太快，但经过$T$步后，

$$\frac{w_f^T}{\|w_f\|}\frac{w^T}{\|w_T\|} \ge \sqrt{T} \cdotp constant$$

![PLA-proof-2](/images/ML/PLA-proof-2.png "PLA-proof-2")

因为$\frac{w_f^T}{\parallel w_f \parallel}\frac{w^T}{\parallel w_T \parallel} \le 1$，所以一定能在有穷步收敛。

其中$constant = \frac{\min \limits_{n} {y_n \frac{w_f^T}{\parallel w_f \parallel} x_n}} {\max \limits_{n} \parallel{x_n}\parallel}$.证明如下:

由（1）可知，经过$T$步之后：

$$w_f^Tw_t \ge w_f^Tw_{t-1} + \min \limits_{n}{y_nw_f^Tx_n} \ge \cdots \ge T\min \limits_{n}{y_nw_f^Tx_n}$$

由（2）可知：

$$\parallel{w_t}^2\parallel \le \parallel{w_{t-1}}^2\parallel + \max \limits_{n} \parallel{x_n}^2\parallel \le \cdots \le T\max \limits_{n} \parallel{x_n}^2\parallel$$

即：

$$\frac{1}{\parallel{w_t}\parallel} \ge \frac{1}{\sqrt{T}\max \limits_{n} \parallel{x_n}\parallel}$$

综合可得：

$$\frac{w_f^Tw_t}{\parallel{w_f^T}\parallel\parallel{w_t}\parallel} \ge \frac{T\min \limits_{n}{y_nw_f^Tx_n}}{\sqrt{T} \parallel{w_f^T}\parallel \max \limits_{n} \parallel{x_n}\parallel} = \sqrt{T}\cdotp\frac{\min \limits_{n} {y_n \frac{w_f^T}{\parallel w_f \parallel} x_n}} {\max \limits_{n} \parallel{x_n}\parallel}$$

## 当数据不是线性可分的时候：Pocket algorithm

当数据中存在许多噪声数据时候，数据不是线性可分，如何处理噪声数据呢？

![PLA-noise](/images/ML/PLA-noise.png "PLA-noise")

Pocket algorithm（口袋算法）是用来解决当数据中存在噪声数据的PLA问题，即找一条能满足大部分的数据的线（超平面），可以看成PLA的变种。

![PLA-pocket-algorithm](/images/ML/PLA-pocket-algorithm.png "PLA-pocket-algorithm")

口袋算法思想：**每当要“修正”一个错误然后修改$w$的时候，都要跟原来的$w$比较，看谁犯的错误更多，如果新的$w$错误更少，就修改，否则不修改。**

口袋算法虽然能够处理噪声数据，但如果数据是可以线性可分的时候，口袋算法和$PLA$算法运行结果相同（因为每次比较肯定都会优于原来），相比较$PLA$算法，口袋算法因为多了比较的步骤，它的运行时间更长。

# python实现

## PLA

**Problem description**: 

Each line of the data set contains one $(x_n, y_n)$ with $x_n \in \mathbb{R}^4$. The first 4 numbers of the line contains the components of $x_n$ orderly, the last number is $y_n$. Please initialize your algorithm with $w = 0$ and take $sign(0)$ as $−1$. As a friendly reminder, remember to add $x_0 = 1$ as always!

[**Solution**](https://github.com/Dervean/machine_learning_foundations/tree/master/Perceptrons_Learning_Algorithm)

## PA

**Problem description**: 

As above.

[**Solution**](https://github.com/Dervean/machine_learning_foundations/tree/master/Pocket_Algorithm)

































