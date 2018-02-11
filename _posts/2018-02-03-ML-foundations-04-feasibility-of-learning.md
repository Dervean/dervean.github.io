---
layout: post
title: "机器学习基石第4课：学习的可行性"
author: Dervean
description: "机器学习基石第4课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/03/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# 机器学习到东西了吗？

机器学习从已知的数据 $D$ 中找到能最接近 $f$ 的 $g$，但因为 $f$ 是未知的，找到的 $g$ 也没办法判断其优劣，即使在部分数据上满足 $g \approx f$，也不能保证在所有数据上符合条件。

那机器学习最终真的学到东西了吗？

如果想要保证机器从已知的数据中学习到东西，必须保证一个前提：**数据集（包括训练集和测试集）必须是从相同分布中产生的。**

机器学习是从**样本数据（sample）中学习到数据的分布。**

![feasibility](/images/ML/feasibility-1.png "feasibility")

# [Hoeffding Inequality](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)

课程中给出一个例子来引出 **Hoeffding Inequality**：

有一个罐子，罐子里面有许许多多的橘色和绿色的弹珠，我们无法将所有弹珠都数一遍，如何知道**橘色弹珠的比例**？

这时候可以利用**抽样**来估计，为了减少误差，需要增大抽样的样本数目 **$N$**.

假如橙色弹珠的真实比例为$\mu$, 而从样本中估计出的比例为$\nu$，样本大小 **$N$**，$\epsilon$ 表示误差范围:

$$P(\mid \nu − \mu \mid > \epsilon) \le 2exp(−2 \epsilon^2 N)$$

其中推论 $\mu = \nu$ 是符合 $PAC$ 理论的(**probably approximately correct**，即大概率可以认为是正确的)。

# 从弹珠到机器学习

我们来做个类比：

绿色弹珠 $\Leftrightarrow$ 模型 $h$ 预测结果与样本真实数据一致.

橘色弹珠 $\Leftrightarrow$ 模型 $h$ 预测结果与样本真实数据不一致.

橘色弹珠的比例即模型 $h$ 的错误率.

样本中的橘色弹珠的比例为 $E_{in}(h)$.

全部弹珠中的橘色弹珠的比例为 $E_{out}(h)$.

根据 **Hoeffding Inequality**：

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2exp(−2 \epsilon^2 N)$$

可以看出: $N$ 越大，$E_{in}(h)$ 和 $E_{out}(h)$， 也就是训练误差和泛化误差越接近.

$E_{in}(h) \approx E_{out}(h)$  **and**  $E_{in}(h)$  **small**  $\Rightarrow$ $E_{out}(h)$  **small**  $\Rightarrow$ $h \approx f$ with respect to P

# Multiple hypothesis

上述描述的是只有一个 $hypothesis$ 的情况，但实际情况可能存在多个 $hypothesis$，这样的话上述结论（$h \approx f$）还成立吗？

![multiple-hypothesis](/images/ML/feasibility-multiple-hypothesis.png "multiple-hypothesis")

当有多个 $hypothesis$ 的时候，存在一个问题: 有很小的概率抽出一些 sample ，使得 $h$ 在样本上得到的误差与在总体上得到的误差相差很大（抽出的样本不能反映总体），课程中将这部分的 sample 称为 bad data，此时不能满足条件 $E_{in}(h) \approx E_{out}(h)$.

如下图所示，如果进行多次抽样，那么肯定有一些样本会导致 $E_{in}(h)$ 和 $E_{out}(h)$ 的差距较大。

![bad-data](/images/ML/feasibilty-bad-data-1.png "bad-data")

因为有多个 $h$，所以可以进一步表示为下图，每个 $h$ 都可能会对应一部分 bad data:

![bad-data](/images/ML/feasibilty-bad-data-2.png "bad-data")

选择出 bad data 的概率:

$$
\begin{array}{rcl}
\mathbb{P}_D(Bad \ D) 	 & 			= 		& \mathbb{P}_D(Bad \ D \ for \ h_1 \ or \ Bad \ D \ for \ h_2 \ or \ ... \ or \ Bad \ D \ for \ h_M) 	\\
						 & 			\le 	& \mathbb{P}_D(Bad \ D \ for \ h_1) + \mathbb{P}_D(Bad \ D \ for \ h_2) + ... + \mathbb{P}_D(Bad \ D \ for \ h_M)		\\
						 &	(union bound) 	&								\\
						 & 			\le 	& 2exp(−2 \epsilon^2 N) + 2exp(−2 \epsilon^2 N) + ... + 2exp(−2 \epsilon^2 N) \\
						 & 			=		& 2Mexp(−2 \epsilon^2 N)	
\end{array}
$$

![bad-data](/images/ML/feasibilty-bad-data-3.png "bad-data")

其概率**只取决于 $M$, $N$, $\epsilon$**.

综上所述:

**如果所有数据都满足同一分布，模型的hypothesis的个数 $M$ 是有限的，样本数目 $N$ 足够大，则能保证 $E_{in}(h) \approx E_{out}(h)$，此时如果再能找到一个$E_{in}(h) \approx 0$，则$E_{out}(h) \approx 0$，从而实现机器学习。**

现在还没有讨论当 $M = \infty$ 该怎么办，例如 $PLA$ 的 $hypothesis$ 的个数无限大的时候。


