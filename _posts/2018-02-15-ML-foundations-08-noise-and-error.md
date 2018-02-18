---
layout: post
title: "机器学习基石第7课：Noise and Error"
author: Dervean
description: "机器学习基石第8课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/15/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Preview

if finite $d_{VC}$, large $N$ and low $E_{in}$ $\Longrightarrow$ learning possible.

前面的课程中并没有考虑到 noise data 的影响，而现实世界中会因为各种原因使数据并**不符合预期**，例如在信用卡发放案例里: 

1. 错误地标记 (mislabel) 顾客的信用好坏；

2. 并不准确的顾客数据；

3. 信息数据完全相同的两个顾客，信用标记却完全相反。（说明影响信用标记的因素比我们搜集的资料要多）

如何处理包含 noise data 的数据呢？前面所说的 VC Dimension 还适用吗？

# Noise and Probabilistic Target

课件里再次使用了橘色、绿色弹珠的比喻来解释 noise data。

前面我们使用罐子里的橘色弹珠表示 $f(x) \neq h(x)$，绿色代表 $f(x) = h(x)$，小球满足 $x \sim P(x)$，小球的颜色是固定的（表示结果确定）。

现在加入了 noise: 小球在抽出来的时候颜色是不固定的，但满足一种 distribution。

一开始我没理解是啥意思，后来想了一下，可能是在说：虽然我抽出来了一个 sample，但我也没法确定我抽出来的球一定是橘色（绿色），它们的颜色是在变的，可是小球分布是确定的（例如橘色 30%，绿色 70%）。

之前的是 ‘deterministic’，$x \sim P(x)$，橘色弹珠 ($f(x) \neq h(x)$) 是确定的 (deterministic)；

现在的是 ‘probabilistic’，$x \sim P(x)$，橘色弹珠 ($f(x) \neq h(x)$) 分布满足 $y \sim P(y \| x)$。

这里可以证明：在 $x \sim P(x)$，$y \sim P(y \| x)$ 时 **VC Bound 仍然成立**。

现在我们可以修改第一章的图，我们之前一直在研究 **target function**，但现在存在 noise data，我们改成研究 **target distribution**。

![noise-and-probabilistic-target](/images/machine-learning-foundations/noise-and-error-noise-and-probabilistic-target-1.png "noise and target distribution")

我们要找出一个 **ideal mini-target** (能满足更多的现实数据，因为我们假定 noise data 的比例不能高于 correct data)。

现在看来: ‘deterministic’ 是 ‘probabilistic’ 的一个 special case of target distribution:

- $P(y \| x) = 1 \ for \ y = f(x)$;

- $P(y \| x) = 0 \ for \ y \neq f(x)$.

# Error Measure

这节介绍了两种度量误差(error measure $E(g,f)$)的方法，两种方法都是 **Pointwise Error Measure**:

$$ E_{out}(g) = \underset{x \sim P}{\varepsilon} \underbrace{\llbracket g(x) \neq f(x) \rrbracket}_{err(g(x),f(x))}  $$

- classification error，又称为 **0/1 error**，

# Algorithm Error Measure

# Weighted Classification






























