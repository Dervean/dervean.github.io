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

**Pointwise Error Measure**:

$$ E_{out}(g) = \underset{x \sim P}{\varepsilon} \underbrace{\llbracket g(x) \neq f(x) \rrbracket}_{err(g(x),f(x))}  $$

$$ E_{in}(g) = \frac{1}{N} \sum_{n=1}^N err(g(x_n),f(x_n)) $$

两种 Pointwise Error Measure:

1. **0/1 error**, classification error (correct or incorrect)

  $$ err(\tilde{y} , y) = \llbracket \tilde{y} \neq y \rrbracket $$

  $$ f(x) = \underset{y \in \mathcal{y}}{argmax} P(y | x) $$

2. **squared error**, regression error (how far is $\tilde{y}$ from y)

  $$ err(\tilde{y} , y) = (\tilde{y} - y)^2 $$

  $$ f(x) = \sum_{y \in \mathcal{y}} y \cdotp P(y | x) $$

# Algorithm Error Measure

如何选择度量误差的方法？

课件里给出一个案例：指纹识别。识别的结果有两种，accpet 和 reject。

我们肯定不希望机器得到的结果是错误的，而判断错误也有两种：一是本应 reject 的却 accpet 了，二是本应 accept 的却 reject 了。

![algorithm-error-measure](/images/machine-learning-foundations/noise-and-error-noise-algorithm-error-measure-1.png "two types of error: false accept and false reject")

判断错误的“惩罚”应该多大呢？

- 超市贵宾打折判断：如果系统将贵宾顾客判断为非贵宾，很可能失去本来应有的顾客，而给非贵宾的顾客折扣并不会有很大的损失，所以这种情况 false reject 的 cost 需要设置很大。

- 国家安全部门：如果系统将非内部人员判断为内部人员，则损失是巨大的，所以 false accept 的 cost 需要设置很大。

从上面看来，不同情况需要使用不同的 Error Measures，我们很难准确地断真实情况的 err 并且用某种公式计算出来，所以使用 $\hat{err}$ 来代替 $err$，如何设计合适的 $\hat{err}$ 将是算法设计的核心。我们希望能减少 $\hat{err}$ 并最终得到 Ideal Mini-Target ($f(x)$)。

# Weighted Classification

将二元分类的 err 加上权重（weight），权重用来衡量 cost for error。

本节主要讨论了二元分类算法在加上 weight 之后应该如何修改源码的问题。

假设 false accept 的 cost 为 1000，false reject 的 cost 为 1。

例如前面课程中我们使用 Pocket Algorithm 代替 PLA，此时加上 weight 如何修改呢？是不是在判断能否用 $w_{t+1}$ 替代 $w_t$ 的时候直接加权重算呢？—— **不行**（不行的原因就不细说了）。

（这里引出一个新的名词 **reduction**）具体解决方法是将 weighted $E_{in}$ **变换**成 0/1 **E_{in}**：将 $y = -1$ 的 data 复制 1000 倍。当然真正实现也不可能真得这样复制（硬盘怕是装不下），我们使用 “virtual copy” —— 频繁地（1000倍）check y = -1 的 data 即可。

































