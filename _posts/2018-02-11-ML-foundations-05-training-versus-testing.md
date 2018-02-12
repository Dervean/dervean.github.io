---
layout: post
title: "机器学习基石第5课：Training versus Testing"
author: Dervean
description: "机器学习基石第5课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/11/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Preview

上节课已经得出结论:

**如果所有数据都满足同一分布，模型的hypothesis的个数 $M$ 是有限的，样本数目 $N$ 足够大，则能保证 $E_{in}(h) \approx E_{out}(h)$，此时如果再能找到一个$E_{in}(h) \approx 0$，则$E_{out}(h) \approx 0$，从而实现机器学习。**

即在 multiple hypothesis 的情况下:

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2 \cdotp M \cdotp exp(−2 \epsilon^2 N)$$

其中有一个限制条件是: $M$ 是有限的。

那如果 $M$ 是无限的情况该怎么考虑，机器学习是否还可行？ 

注意上述公式推导选择 bad data 的概率时:

$$
\begin{array}{rcl}
\mathbb{P}_D(Bad \ D) 	 & 			= 		& \mathbb{P}_D(Bad \ D \ for \ h_1 \ or \ Bad \ D \ for \ h_2 \ or \ ... \ or \ Bad \ D \ for \ h_M) 	\\
						 & 			\le 	& \mathbb{P}_D(Bad \ D \ for \ h_1) + \mathbb{P}_D(Bad \ D \ for \ h_2) + ... + \mathbb{P}_D(Bad \ D \ for \ h_M)		\\
						 &	(union \ bound) &								\\
						 & 			\le 	& 2exp(−2 \epsilon^2 N) + 2exp(−2 \epsilon^2 N) + ... + 2exp(−2 \epsilon^2 N) \\
						 & 			=		& 2 \cdotp M \cdotp exp(−2 \epsilon^2 N)	
\end{array}
$$

其中使用的是 union bound. 即假设每个 hypothesis 的 bad data 的分布没有互相重叠，然而现实情况下这种条件并不能满足，这样看来这个 upper bound 应该是被高估(**over-estimating**)了。

例如 PLA , 虽然 hypothesis 的数目无穷，但相似的两个 hypothesis 的 bad data 的分布也是会有许多重叠的情况。

在这种情况下，我们修改一下上面的公式，考虑将无穷多个 hypothesis 分成若干类 $m_H$ ，使用 $m_H$ 来代替 **infinite M**，hypothesis 的种类个数与 input 有关，所以 $m_H$ 是一个关于 input 个数的单调递增函数:

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2 \cdotp m_H \cdotp exp(−2 \epsilon^2 N)$$

# Effective Number of Line

如何将无穷多个 hypothesis 分成若干类 $m_H$ ？

讲义中以一个例子（其实就是 PLA 的情况）解释了什么是 **Effective Number**:

- 假设平面上有一些点，现在用一根直线把这些点分成两类（+1、-1），请问这根直线最多有几种。

- 如果只有 1 个点 $x_1$，那么直线有 2 种：一种将 $x_1$ 划为+1，另一种将 $x_1$ 划为-1。（$2^1$）

- 如果有 2 个点 $x_1$，$x_2$，那么直线有 4 种: $x_1$ 和 $x_2$ 都为+1，$x_1$ 和 $x_2$ 都为-1，$x_1$ 为+1 $x_2$ 为 -1，$x_2$ 为+1 $x_1$ 为 -1。（$2^2$）

- 如果有 3 个点 $x_1$，$x_2$，$x_3$，注意此时可能三点共线，所以直线**最多 8 种**。（$2^3$）

- 但是当有 4 个点的时候，情况出现了变化，此时直线**最多 14 种**。此时的 4 是 $m_H$ 的 **break point**。（$14 < 2^4$）

![effective-number-of-line](/images/ML/training-versus-testing-1.png "当有四个点时，最多有14种直线将这些点分成两类")

我们将直线的种类数目称为 Effective Number of Line，Effective Number 意味着线虽无穷但类别有穷，即:

effective(N) $\Leftrightarrow$ maximum kinds of lines with respect to N inputs $x_1$, $x_2$, ..., $x_N$

进一步可得:

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2 \cdotp effective(N) \cdotp exp(−2 \epsilon^2 N)$$

其中 $effective(N) \le 2^N$

**如果 $effective(N)$ 可以替代 M，且 $effective(N) \ll 2^N$，则学习可行。**

# Effective Number of Hypotheses

这里引出一个新的名词: $dichotomy$. 中文意思是“二分”，意思是将空间中的点（例如二维平面）用一条直线分成正类（蓝色o）和负类（红色x）。

空间里面有 N 个点，每个 hypothesis 是可以将这些点分开的直线，令 H 是所有 $hypothesis$ 的集合，而 $dichotomy(H)$ 代表着 hypothesis 的种类数目。可以知道 $\|H\|$ 可能是无穷的，但 $dichotomy(H) \le 2^N$.

我们将之前引入的 $m_H(N)$ 称为**成长函数(growth function)**，指的是对应不同的 N 时 hypothesis 的最大的种类数目，也就是最大的 $dichotomy(H)$，易知 $m_H(N) \le 2^N$.

讲义中又介绍了三种成长函数:

(1) Positive Rays

连续 N 个点分布在一条一维直线上，现在要从 N+1 个间隔选择出一个将 N 个点分成两类。

$$m_H(N) = N + 1$$

![effective-number-of-hypotheses](/images/ML/training-versus-testing-2.png "Positive Rays")

(2) Positive Intervals

连续 N 个点分布在一条一维直线上，根据 N+1 个间隔选择出一段点（interval），将 N 个点分成两类，处于 interval 上的点属于 +1，处于 interval 外的点属于 -1。

$$m_H(N) = C_{N+1}^2 + 1 = \frac{1}{2}N^2 + \frac{1}{2}N + 1$$

![effective-number-of-hypotheses](/images/ML/training-versus-testing-3.png "Positive Intervals")

(3) Convex Sets

平面上 N 个点，求 convex polygon 的个数的成长函数。

$$m_H(N) = 2^N$$

![effective-number-of-hypotheses](/images/ML/training-versus-testing-4.png "Convex Sets")

新的名词: **shatter**

$m_H(N) = 2^N$ $\Leftrightarrow$ exists N inputs that can be shattered

# Break point

**Break point**: if no k inputs can be shattered by $H$, call k a **break point** for $H$.

* 每种 hypothesis 对应的成长函数以及 break point

    |---
    | Hypothesis | Growth function | break point
    |-|:-|:-
    | Positive Rays | $m_H(N) = N + 1$ | k = 2
    | Positive Intervals | $m_H(N) = C_{N+1}^2 + 1 = \frac{1}{2}N^2 + \frac{1}{2}N + 1$ | k = 3
    | Convex Sets | $m_H(N) = 2^N$ | no break point
    | 2D perceptrons | $m_H(N) < 2^N$ | k = 4

由上面可以猜测:

- no break point:	$m_H(N) = 2^N$

- break point	:	$m_H(N) = O(N^{k-1})$
















