---
layout: post
title: "机器学习基石第14课：Regularization"
author: Dervean
description: "机器学习基石第14课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/03/11/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Regularized Hypothesis Set

Regularization 是帮助我们解决过拟合问题的一种方法。例如在[第13课](https://dervean.github.io/blog/2018/03/11/ML-foundations-13-hazard-of-overfitting/)中所提到的 10 阶多项式过拟合问题，如果剔除高阶多项式的项，而只保留 2 阶多项式的项:

![regularization-1](/images/machine-learning-foundations/regularization-1.png "只保留 2 阶多项式的项")

在[第12课](https://dervean.github.io/blog/2018/03/06/ML-foundations-12-nonlinear-transformation/)讨论非线性变换的时候我们曾经提到对于多项式 hypothesis set 有一个 Structured 的特征，即高阶的 hypothesis set 包含低阶的 hypothesis set:

![regularization-2](/images/machine-learning-foundations/regularization-2.png "Structured Hypothesis Sets")

我们将 10 阶多项式的高阶（3～10次）多项式的系数全部设为 0，完成一次从高阶到低阶的转换，hypothesis 变得更加平滑，过拟合问题也得到解决。

现在我们调整一下限制，从保留 0～2 阶多项式系数改为保留最多任意三个非零系数:

$$
\sum_{q=0}^{10}[\![ w_q \neq 0 ]\!] \le 3
$$

现在得到的 hypothesis set 设为 $\mathcal{H}_2'$，称为 sparse hypothesis set:

$$
\mathcal{H}_2 \subset \mathcal{H}_2' \subset \mathcal{H}_{10} 
$$

然而求解 sparse hypothesis set 问题被证明是 NP-hard 的。

现在我们改变一下限制条件，改为限制 hypothesis $w$ 的大小，即:

$$
\sum_{q=0}^{10}w_q^2 \le C
$$

这样得到的 hypothesis 称为 $\mathcal{H}(C)$，得到的 $w$ 称为 $w_{REG}$（regularized hypothesis），$C$ 越大则表示限制越小，$\mathcal{H}(C)$ 与 $\mathcal{H}_2'$ 有交集但非完全一样，例如当 $w$ 的每个分量都很小且非零，则 $\mathcal{H}(C)$ 包含而 $\mathcal{H}_2'$ 不包含。

$$
\mathcal{H}(0) \subset \mathcal{H}(1.126) \subset ... \subset \mathcal{H}(1126) \subset ... \subset \mathcal{H}(\infty) = \mathcal{H}_{10}
$$

# Weight Decay Regularization 

如何求解 $w_{REG}$ ？

既然对 $E_{in}$ 加了限制条件:

$$
\begin{array}{rl}
\underset{w \in \mathbb{R}^{Q+1}}{min} & E_{in}(w) = \frac{1}{N} \underbrace{\sum_{n=1}^N \left( w^T z_n -y_n\right)^2}_{(Zw-y)^T(Zw-y)} \\
s.t. & \underbrace{\sum_{q=0}^Q w_q^2}_{w^Tw} \le C
\end{array}
$$

$\underbrace{\sum_{q=0}^Q w_q^2}_{w^Tw} \le C$ 是一个超球体(hypersphere)，半径为 $\sqrt{C}$，$w$ 被限制在这个超球体的内部。以上写成矩阵形式:

$$
\begin{array}{rl}
\underset{w \in \mathbb{R}^{Q+1}}{min} & E_{in}(w) = \frac{1}{N}(Zw-y)^T(Zw-y) \\
s.t. & w^Tw \le C
\end{array}
$$

如下图所示，根据梯度下降算法，$w$ 会朝着 $−\triangledown E_{in}$ 的方向移动，如果没有限制，$w$ 最终会取得“谷底”最小值 $w_{lin}$。现在我们将 $w$ 限制在半径为 $\sqrt{C}$ 的超球体内（即 $w^Tw = C$ ），$w$ 无法取球外的任何值。当 $w_{Lin}$ 不在球内时，则$w$ 最大只能位于球面上，$w$ 变动的方向（即梯度方向）可以分解为切线方向和法向量方向，但因为受到超球体的限制，所以法向量部分被舍弃，$w$ 沿着球面的切线方向移动，随着迭代过程不断进行，等到切线方向的分量为0，即梯度与 $w$ 平行，$w$ 更新结束，得到最终解。

![regularization-3](/images/machine-learning-foundations/regularization-3.png "梯度下降，但受到超球面的限制")

既然最终状态是梯度方向与 $w$ 方向平行:

$$
\triangledown E_{in}(w_{REG}) + \frac{2\lambda}{N}w_{REG} = 0 
$$

其中 $\frac{2}{N}$ 只是为了方便后面的推导，$\lambda$ 是拉格朗日乘子，从而得到:

$$
\frac{2}{N}\left(Z^TZw_{REG} - Z^Ty\right) + \frac{2\lambda}{N}w_{REG} = 0
$$

解得:

$$
w_{REG} \longleftarrow (Z^TZ + \lambda I)^{-1}Z^Ty
$$

如果给定 $\lambda$，带入上式即可计算出 $w_{REG}$，当然确定 $\lambda$ 也是需要根据具体的情况进行分析调试。

现在我们从另一方面考虑 $\triangledown E_{in}(w_{REG}) + \frac{2\lambda}{N}w_{REG} = 0$，在没有加入限制条件时我们希望 $E_{in}$ 越小越好，现在加入了限制条件，我们对等式左边积分，而等式右边的 0 则可以看作取 minimize，即:

$$
\text{min }\underbrace{E_{in}(w) + \frac{\lambda}{N} w^Tw}_{\text{augmented error }E_{aug}(w)}
$$

现在这个 $E_{aug}$ 是新的 $E_{in}$，我们的目标是将它最小化，其中的 $\frac{\lambda}{N} w^Tw}$ 就是 regularization，称作 **Weight Decay Regularization**，看看不同的 $\lambda$ 对结果的影响:

![regularization-4](/images/machine-learning-foundations/regularization-4.png "results")

$\lambda$ 可以看作是对模型复杂度的惩罚，使得模型变得更加平滑（当然太大也会使得模型变得欠拟合），总的来说，$\lambda$ 越大，$C$ 越小，$w$ 越小:

$$
\begin{array}{rl}
& \text{larger } \lambda \\
\Longleftrightarrow & \text{prefer shorter } w \\
\Longleftrightarrow & \text{effectively smaller } C
\end{array}
$$

这里其实还要考虑一个问题，现在我们都是简单使用 $x,x^2,x^3...$ 作为变换基，但这种形式的多项式在 $x\in{[-1,+1]}$ 时导致 $x$ 的高阶多项式的值远小于低阶 $x$ 多项式，那么高阶的 $w$ 分量将会很大，为了避免这种情形，可以使用 [Legendre Polynomials](https://en.wikipedia.org/wiki/Legendre_polynomials)。

# Regularization and VC Theory

我们做个对比:

$$
E_{aug}(w) = E_{in}(w) + \frac{\lambda}{N} w^Tw \\
E_{out}(w) \le E_{in}(w) + \Omega(\mathcal{H}) 
$$

regularizer（$w^Tw$）代表着单一 hypothesis 的复杂度，而 $\Omega(\mathcal{H})$ 代表 hypothesis set 的复杂度，即 $E_{aug}$ 应该比 $E_{in}$ 更接近 $E_{out}$。

# General Regularizers

以上我们讨论的 regularizer 都是平方和（$w^Tw$），即限制条件是一个超球体，但是我们可能需要根据实际情况选择不同的 regularizer。如何选择？

计算平方和的 regularizer 又称为 L2 Regularizer，物理意义是一个超球体，特点: convex，differentiable，easy to optimize:

$$
\Omega(w) = \sum_{q=0}^Q w_q^2 = \| w\| ^2
$$

还有 L1 Regularizer，物理意义是一个超多变体，特点: convex, not differentiable everywhere（顶点处不可微），sparsity in solution（顶点处的 $w$ 的许多分量为零），计算速度快（可以用于手机移动端等对计算速度要求快而结果不必太精确的场景）:

$$
\Omega(w) = \sum_{q=0}^Q |w_q| = \| w\| 
$$

一般来说，noise 越大（stochastic noise 和 deterministic noise），需要的 $\lambda$ 也越大。

但如何选取合适的 $\lambda$，在下一节课会继续讨论。









