---
layout: post
title: "机器学习基石第6课：Theory of Generalization"
author: Dervean
description: "机器学习基石第6课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/12/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Preview

上节课指出对于无穷多个 hypothesis 的情况下我们可以使用 **$m_H$** 代替 $M$，并介绍了什么是成长函数以及 break point。

本节课是证明**存在 break point 时，成长函数满足多项式增长，机器学习是可行的**。

# Restriction of Break Point

从上节课的小例子我们得出对于 2D 的 perceptron 的 break point = 4，即当有 4 个点的时候我们没有办法找到足够多种的 dichotomy ($< 2^4$)来 shatter 这 4 个点。

本节课通过一个简单的例子来说明 break point 对成长函数增长的影响。

- 首先，对于一个点我们肯定可以找到两种 dichotomy 使得这个点要么是 +1 要么是 -1，即 $m_H(N) \ge 2$ 

- 假设我们没办法 shatter 住任意两个点，即 minimum break point k = 2，当 N = 2 的时候，$m_H(N) < 4$，不妨令 $m_H(N) = 3$。

- 基于上面两个条件，我们现在来看 N = 3，k = 2 最多有多少个 dichotomy.

  - 首先，在不 shatter 任意两个点的前提下，肯定能存在 1 个 dichotomy.

  ![1-dichotomy](/images/ML/theory-of-generalization-1.png "1 dichotomy")

  - 存在 2 个dichotomy.

  ![2-dichotomy](/images/ML/theory-of-generalization-2.png "2 dichotomy")

  - 存在 3 个dichotomy.

  ![3-dichotomy](/images/ML/theory-of-generalization-3.png "3 dichotomy")

  - 现在我们来研究是否存在 4 个 dichotomy. 首先，确实存在 4 个 dichotomy 且能满足不会 shatter 任意两个点。

  ![4-dichotomy-no-shatter](/images/ML/theory-of-generalization-4.png "不 shatter 任意两个点，存在 4 个 dichotomy")

  - 但也会存在 4 个 dichotomy 但是 shatter 其中两个点的情况。

  ![4-dichotomy-shatter](/images/ML/theory-of-generalization-5.png "4 个 dichotomy，shatter 其中两个点 x2、x3")

  - 然而我们没办法在不 shatter 任意两个点的前提下找到 5 个 dichotomy.

  ![5-dichotomy-shatter-1](/images/ML/theory-of-generalization-6.png "5 个 dichotomy，shatter 其中两个点 x1、x3")

  ![5-dichotomy-shatter-2](/images/ML/theory-of-generalization-7.png "5 个 dichotomy，shatter 其中两个点 x1、x2")

  - 所以 N = 3，k = 2 最多有 4 个 dichotomy （$4 < 2^3$），可以看出来，break point 对成长函数的增长有很大的影响（本来应该是 $2^3$，现在只有 4）。

下面我们来证明 存在 break point 时，$m_H(N)$ 是多项式型的。

# Bounding Function

bounding function $B(N,k)$:  maximum possible $m_H(N)$ when break point = k.

定义这样一个函数是为了研究 $m_H(N)$ 的上界以及增长情况，现在我们来证明 $B(N,k) \le poly(N)$.

- 由上面的例子分析可以知道: $B(2,2) = 3$, $B(2,2) = 4$.

- 当 N < k 的时候，$B(N,k) = 2^N$，因为 break point = k，所以一定可以 shatter 住 $N \le k-1$ 个点。

- 当 k = 1 时，$B(N,k) = 1$，连一个点都 shatter 不住了，更别提更多的点了。

- 当 N = k 时，$B(N,k) < 2^k$，不妨取上界 $B(N,k) = 2^k - 1$

所以可以得到下面的表:

![bounding-function-basic-case](/images/ML/theory-of-generalization-bounding-function-basic.png "bounding function —— basic case")

下一步我们通过归纳（inductive）来补全这张表:

首先我们可以暴力计算出: $B(4,3) = 11$

![bounding-function-inductive-case-1](/images/ML/theory-of-generalization-bounding-function-inductive-1.png "B(4,3) = 11")

11 = 4 + 7 ？表中元素之间是否有某种关系 ？下面来进一步研究 $B(4,3)$ 的组成。

![bounding-function-inductive-case-2](/images/ML/theory-of-generalization-bounding-function-inductive-2.png "B(4,3)")

重新排列一下

![bounding-function-inductive-case-3](/images/ML/theory-of-generalization-bounding-function-inductive-3.png "B(4,3)")

图中的 dichotomy 分成橘色和紫色两个部分，橘色的部分两两成双，每一对的 dichotomy 在 $x_1, x_2, x_3$ 上的分量相同，我们把 $x_1, x_2, x_3$ 不同的部分提取出来

![bounding-function-inductive-case-4](/images/ML/theory-of-generalization-bounding-function-inductive-4.png "B(4,3)")

橘色部分的分量的个数记为 $\alpha$，紫色的部分的个数记为 $\beta$，可以得到: 

$$B(4,3) = 2 \cdotp \alpha + \beta$$

又因为在任意三个点上都不会 shatter，所以 

$$\alpha + \beta \le B(3,3)$$

只观察橘色的部分，$x_1, x_2, x_3$ 中任意两点不能 shatter: 假如能够 shatter，例如 $x_1, x_2$ 能够 shatter，则 $x_1, x_2, x_4$ 也能 shatter，矛盾。所以能得到

$$\alpha \le B(3,2)$$

综上所述:

$$
\begin{array}{rcl}
B(4,3)     &          =       & 2 \cdotp \alpha + \beta   \\
           &          \le     & B(4,3) + B(3,2)  
\end{array}
$$

递推可得:

$$
\begin{array}{rcl}
B(N,k)                      &          =       & 2 \cdotp \alpha + \beta    \\
\alpha + \beta              &          \le     & B(N-1,k)                   \\
\alpha                      &          \le     & B(N-1,k-1)                 \\
\Rightarrow     B(N,k)      &          \le     & B(N-1,k) + B(N-1,k-1)
\end{array}
$$

这样就可以将表补全:

![bounding-function-inductive-case-5](/images/ML/theory-of-generalization-bounding-function-inductive-5.png "inductive case")

$$B(N,k) \le \underbrace{\sum_{i=0}^{k-1} \dbinom{N}{i}}$$















