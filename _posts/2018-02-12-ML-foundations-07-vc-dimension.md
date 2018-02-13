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

对比 $B(N,k)$ 和 $N{k-1}$:

![vc-dimension-definition-1](/images/ML/vc-dimension-definition-1.png "vc dimention definition")

可以看出两点:

- 当 $N \ge 2$，$k \ge 3$ 时，$B(N,k) \le N{k-1}$.

- 随着 N 的增长，$N{k-1}$ 的增长速度（特别是当 k 比较大的时候）远超过 $B(N,k)$.

下面我们来回顾一下 VC bound:

For any $g = \mathcal{A}(D) \in \mathcal{H}$ and 'statistical' large $\mathcal{D}$, $k \ge 3$:

$$
\begin{array}{rcl}
  		&	\mathbb{P}(| E_{in}(g) - E_{out}(g) | > \epsilon) 										\\
\le 	&	\mathbb{P}(\exists h \in \mathcal{H} \ s.t. \ | E_{in}(g) - E_{out}(g) | > \epsilon) 	\\
\le 	&	4m_{\mathcal{H}}(2N)exp(-\frac{1}{8} \epsilon^2 N) 										\\
if k exists																							\\
\le 	& 4(2N)^{k-1} exp(-\frac{1}{8} \epsilon^2 N)
\end{array}
$$


















