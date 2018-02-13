---
layout: post
title: "机器学习基石第7课：VC dimension"
author: Dervean
description: "机器学习基石第7课"
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

回顾上节课所说的内容，可以得到下列的结论:

if $m_{\mathcal{H}}$ **breaks** somewhere and **N large enough**  $\Leftrightarrow$  $E_{out} \approx E_{in}$ possible.

这节课主要回答以下问题:

- $E_{out}$ 和 $E_{in}$ 到底有多接近

- $E_{out}$ 和 $E_{in}$ 接近的程度与 N 和 break point k 有什么联系

- 在实际的学习中如何选取 $\mathcal{H}$，需要多大的数据量 $N$.

# Definition of VC dimension

上节课给出了 growth function 的 upper bound:

如果 $m_{\mathcal{H}}(N)$ 有 break point k:

$$ m_{\mathcal{H}}(N) \le \underbrace{\sum_{i=0}^{k-1} \binom{N}{i}}_{highest \ term \ N^{k−1}}$$

对比 $B(N,k)$ 和 $N^{k-1}$:

![vc-dimension-definition-1](/images/ML/vc-dimension-definition-1.png "vc dimention definition")

可以看出两点:

- 当 $N \ge 2$，$k \ge 3$ 时，$B(N,k) \le N^{k-1}$.

- 随着 N 的增长，$N^{k-1}$ 的增长速度（特别是当 k 比较大的时候）远超过 $B(N,k)$.

下面我们来回顾一下 VC bound:

For any $g = \mathcal{A}(\mathcal{D}) \in \mathcal{H}$ and 'statistical' large $\mathcal{D}$, for $N \ge 2$, $k \ge 3$:

$$
\begin{array}{rcl}
\mathbb{P}(| E_{in}(g) - E_{out}(g) | > \epsilon) 	& 	\le 	&	\mathbb{P}(\exists h \in \mathcal{H} \ s.t. \ | E_{in}(g) - E_{out}(g) | > \epsilon) 	\\
													& 	\le 	&	4m_{\mathcal{H}}(2N)exp(-\frac{1}{8} \epsilon^2 N) 										\\
													& 	if \ k \ exists																						\\
													& 	\le 	& 4(2N)^{k-1} exp(-\frac{1}{8} \epsilon^2 N)
\end{array}
$$

从上面来看，学习可行需要有三个条件:

- good $\mathcal{H}$: $m_{\mathcal{H}}(N)$ breaks at k.

- good $\mathcal{D}$: N large enough $\Rightarrow$ probably generalized '$E_{in} \approx E_{out}$'.

- good $\mathcal{A}$: $\mathcal{A}$ picks a $g$ with small $E_{in}$ $\Rightarrow$ probably learned.

而 VC Dimension 的定义就是上面的  **k-1**  . 代表着 **maximum non-break point**.

**Definition**: VC dimension of $\mathcal{H}$, denoted $d_{VC}(\mathcal{H})$ is **largest N** for which $m_{\mathcal{H}}(N) = 2^N$.

- the **most** inputs $\mathcal{H}$ that can shatter.

- $d_{VC}$ = 'minimum k' - 1.

- $N \le d_{VC} \Rightarrow H$ can shatter some $N$ inputs. 
  对应矩阵的上三角部分，注意这里并不是对于任意 N 个 input 都能 shatter。所以当存在一组 N 个 input 能被 shatter 的时候也不能断定 $N \le d_{VC}$，当存在一组 N 个 input 不能被 shatter 的时候也不能断定 $N > d_{VC}$.

- $N > d_{VC} \Rightarrow k$ is a break point for $\mathcal{H}$. 
  对于所有的整数 $k > d_{VC}$ 都是 $\mathcal{H}$ 的 break point.

现在使用 VC dimension 代替了 k:

if $N \ge 2$, $d_{VC} \ge 2$, $m_{\mathcal{H}}(N) \le N^{d_{VC}}$.

提出 VC dimension 的重要在于：如果我们能够找到一个有限的$d_{VC}$，则一定能 generaliz 一个 $g$ 使得 $E_{in}(g) \approx E_{out}(g)$。

注意这个结论**与选择什么学习算法无关，与输入是什么分布无关，与目标函数是什么无关**。



















