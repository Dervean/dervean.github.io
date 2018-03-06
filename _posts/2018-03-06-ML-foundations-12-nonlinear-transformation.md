---
layout: post
title: "机器学习基石第12课：Nonlinear Transformation"
author: Dervean
description: "机器学习基石第12课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/03/06/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Quadratic Hypotheses

之前几节课介绍的都是线性模型，但当数据不是线性可分，强行使用线性模型会造成 $E_{in}$ 很大随之 $E_{out}$ 也很大，那该怎么办呢？—— 可以考虑将问题从非线性空间映射到线性空间。

![nonlinear-transformation-1](/images/machine-learning-foundations/nonlinear-transformation-1.png "Quadratic Hypotheses")

例如上图所示，可以使用一个圆圈将数据很好地分开，此时数据不是 linear seperable 而是 **circlar seperable**，如果**将圆心作为原点**，圆心半径为 $\sqrt{0.6}$:

$$
h_{SEP}(x) = sign(-x_1^2 - x_2^2 + 0.6)
$$

该公式可以看作是非线性空间 $\mathcal{X}$ 向线性空间 $\mathcal{Z}$ 的映射:

$$
\begin{array}{rcl}
h(x) & = & sign \left( \underbrace{0.6}_{\tilde{w}_0} \cdot \underbrace{1}_{z_0} + \underbrace{(-1)}_{\tilde{w}_1} \cdot \underbrace{x_1^2}_{z_1} + \underbrace{(-1)}_{\tilde{w}_2} \cdot \underbrace{x_2^2}_{z_2} \right) \\
     & = & sign(\tilde{w}^T z)
\end{array}
$$

也就是特征转换，Nonlinear Feature Transform $\phi$: 

$$
x \in \mathcal{X} \stackrel{\phi}{\longmapsto} z \in \mathcal{Z}
$$

这里要注意的是：当数据在 $\mathcal{X}$ 上圆形可分，则数据在 $\mathcal{Z}$ 上线性可分；但反之并不成立:

$$
\text{circlar separable in } \mathcal{X} \stackrel{\rightarrow}{\nleftarrow} \text{ linear separable in } \mathcal{Z}
$$

因为映射后的 $\mathcal{Z}$ 得到的 hypothesis 并不只有圆，还有椭圆、双曲线、抛物线甚至常数(取决于 $\tilde{w}$)。

再考虑一般情况，如果圆心不过原点，则 $\phi$ 应该包含常数和所有的一次项以及二次项:

$$
\phi(x) = (1,x_1,x_2,x_1^2,x_1x_2x_2^2)
$$

这样总结得出映射后的空间 $\mathcal{Z}$ 中得到的 hypothesis:

$$
\mathcal{H}_{\phi} = \{h(x) : h(x) = \tilde{h}(\phi(x)) \text{ for some linear } \tilde{h} \text{ on } \mathcal{Z}\}
$$

# Nonlinear Transform

为了寻找到在 $\mathcal{X}\text{-space}$ 上的一个好的 Quadratic Hypothesis，只需要进行**特征转换**和**训练线性模型**，训练完成之后再作替换 $g(x) = sign(\tilde{w}^T\phi(x))$。

![nonlinear-transformation-2](/images/machine-learning-foundations/nonlinear-transformation-2.png "Nonlinear Transform")

# Price of Nonlinear Transform

当 $\mathcal{X}$ 维度为 2，那么 $\mathcal{Z}$ 维度为 6:

$$
(1,x_1,x_2,x_1^2,x_1x_2x_2^2)
$$

当 $\mathcal{X}$ 维度为 $d$，那么对于二阶多项式，即 $\mathcal{Z}$ 维度为:

$$
\tilde{d} = 1 + C_d^1 + C_d^2 + d
$$

但实际中我们并没有办法确定这个“多项式的阶数”。

现在设阶数为 $Q$，则 $\mathcal{Z}$ 维度为:

$$
\tilde{d} = C_{Q+d}^{Q} = C_{Q+d}^{d} = O(Q^d)
$$

当 $Q$ 和 $d$ 比较大的时候，计算的时间、空间复杂度会变得很大，而且 VC dimension 也会变得很大（自由度个数指数级增加），泛化能力变弱（过拟合）。

现在可以看出来出现了一个问题：如何选择 $Q$，使得 $E_{in}$ 能够足够的小但同时不会导致过拟合。

**注意不要人为减少一些特征，避免人为选择训练样本，要保存所有的多项式特征。**

# Structured Hypothesis Sets

随着映射阶数的增加，$\mathcal{Z}\text{-space}$ 中的特征数目是不断增加的，即 VC-dimension 不断增加，而 $E_{in}$ 则是不断减小（拟合越来越好）。

随着 $Z\text{-space}$ 的阶数不断增加:

$$
\begin{array}{rcl}
\phi_0(x) & = & (1) \\
\phi_1(x) & = & (\phi_0(x),x_1,x_2,...,x_d) \\
\phi_2(x) & = & (\phi_1(x),x_1^2,x_1x_2,...,x_d^2) \\
... \\
\phi_Q(x) & = & (\phi_{Q-1}(x),x_1^Q,x_1^{Q-1}x_2,...,x_d^Q)
\end{array}
$$

可以看出：对于不同阶数的 hypothesis，呈现出一种 nested hypothesis structure:

$$
H_{\phi_0} \subset H_{\phi_1} \subset H_{\phi_2} \subset ... \subset H_{\phi_Q}  
$$

即:

$$
\begin{matrix}
\mathcal{H}_0	&	\subset	&	\mathcal{H}_1	&	\subset	&	\mathcal{H}_2	&	\subset ... \\
d_{VC}(\mathcal{H}_0)	&	\le	&	d_{VC}(\mathcal{H}_1)	&	\le	&	d_{VC}(\mathcal{H}_2)	&	\le ... \\
E_{in}(g_0)	&	\ge	&	E_{in}(g_1)	&	\ge	&	E_{in}(g_2)	&	\ge ...
\end{matrix}
$$

![vc-dimension-interpreting-1](/images/machine-learning-foundations/vc-dimension-interpreting-1.png "阶数不能太高，会导致 model complexity 和 outer sample error 增加")

阶数越高模型的自由度越高，泛化能力会随着自由度的增加先增强后减弱（表现为 $E_{out}$ 先减小后增加）。

所以一般我们更加倾向于选择低阶的 hypothesis，根据 $E_{in}$ 的大小不断增加阶数，直到 $E_{in}$ 已经足够小即可。













