---
layout: post
title: "机器学习基石第13课：Hazard of Overfitting"
author: Dervean
description: "机器学习基石第13课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/03/11/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# What is Overfitting

过拟合问题在前面的课程中已经有提及。

![vc-dimension-interpreting-1](/images/machine-learning-foundations/vc-dimension-interpreting-1.png "VC Dimension")

如上图所示，当 VC Dimension 过大，导致虽然 $E_{in}$ 很小，但是 model complexity 和 $E_{out}$ 很大，表现为虽然在 sample 数据拟合得很好，但是模型的泛化能力很弱（bad generalization）。

如下图所示，蓝色曲线是 target function，红色曲线虽然完全拟合了 sample data，但是造成了 overfitting。

![hazard-of-overfitting-1](/images/machine-learning-foundations/hazard-of-overfitting-1.png "overfitting")

影响过拟合的三个因素:

- VC Dimension 

- Noise

- 训练样本数量 N

# The Role of Noise and Data Size 

现在进一步研究过拟合:

1. 包含 noise

  假设 target function 是一个包含噪声数据的关于 $x$ 的 10 阶多项式，现在有两个选择，一是选择一个 10 阶模型，二是选择一个 2 阶模型，哪个会比较好？

  ![hazard-of-overfitting-2](/images/machine-learning-foundations/hazard-of-overfitting-2.png "一个包含噪声数据的 10 阶多项式")

  既然目标函数是 10 阶多项式，感觉可能 10 阶模型更符合要求，但是事实上 **10 阶模型的泛化能力要弱得多**:

  ![hazard-of-overfitting-3](/images/machine-learning-foundations/hazard-of-overfitting-3.png "对比下来 2 阶模型泛化能力要强")

2. 不包含 noise

  假设 target function 是一个不含噪声数据的关于 $x$ 的 50 阶多项式，现在有两个选择，一是选择一个 10 阶模型，二是选择一个 2 阶模型，哪个会比较好？

  ![hazard-of-overfitting-4](/images/machine-learning-foundations/hazard-of-overfitting-4.png "一个包含噪声数据的 10 阶多项式")

  这次既然没有噪声数据，而且目标函数是 50 阶多项式，那是不是可以选择 10 阶模型呢？事实上 **10 阶模型的泛化能力还是要弱得多**:

  ![hazard-of-overfitting-5](/images/machine-learning-foundations/hazard-of-overfitting-5.png "对比下来 2 阶模型泛化能力要强")

简单的学习模型反而能表现的更好。

现在我们来分析其中原因。前面[第 9 课](https://dervean.github.io/blog/2018/02/18/ML-foundations-09-linear-regression/)在介绍 linear regression 的时候，我们给出了 learning curve:

![hazard-of-overfitting-6](/images/machine-learning-foundations/hazard-of-overfitting-6.png "2 阶模型的 learning curve")

这里再给出 10 阶模型的 learning curve:

![hazard-of-overfitting-7](/images/machine-learning-foundations/hazard-of-overfitting-7.png "10 阶模型的 learning curve")

对比可得，在数据量比较少的时候，相比 2 阶模型，10 阶模型的泛化能力是很弱的。

要注意这种现象与 noise 的有无并**没有必然的联系**，即使没有噪声数据，10 阶模型因为其更高的模型复杂度，导致很难通过有限的样本数据得到 target function，从而泛化能力变弱。**模型复杂度也可以看作是一种 noise。**

# Deterministic Noise

设:

$$
\begin{array}{rcl}
y & =    & f(x) + \epsilon \\
  & \sim & Gaussian(\underbrace{\sum_{q=0}^{Q_f} \alpha_q x^q}_{f(x)},\sigma^2)
\end{array}
$$ 

模型复杂度（即多项式次数）为 $Q_f$，含有的噪声数据 $\epsilon$ 服从高斯分布（方差为 $\sigma^2$），数据量为 $N$:

- 当 $N$ 和 $Q_f$ 固定，改变 $\sigma^2$

  ![hazard-of-overfitting-8](/images/machine-learning-foundations/hazard-of-overfitting-8.png "stochastic noise")

- 当 $N$ 和 $\sigma^2$ 固定，改变 $Q_f$ 

  ![hazard-of-overfitting-9](/images/machine-learning-foundations/hazard-of-overfitting-9.png "deterministic noise")

颜色越深代表 overfitting 的程度越大，证明模型的 noise 和模型的复杂度都可以影响到 overfitting，我们称其中的噪声数据为 stochastic noise，称模型复杂度为 deterministic noise。


# Dealing with Overfitting

如何处理过拟合:

- start from simple model $\longrightarrow$ deterministic noise $\downarrow$

  在前面的解释中可以看出，给定有限样本数据，简单的模型的泛化能力可能要比复杂的模型强得多。

- data cleaning/pruning $\longrightarrow$ stochastic noise $\downarrow$

  data cleaning，修正错误的数据；data pruning，去除噪声数据。这个方法困难在于如何找到错误点和噪声点，当样本数据量远大于错误/噪声数据的时候，这种方法效果不是特别明显。
  
- data hinting $\longrightarrow$ N $\uparrow$

  在给定样本数据的情况下，我们希望能够得到更多的训练数据，可以通过对给定数据**做某些变换处理**，例如在数字识别问题里，对已有的数字图片进行平移或者旋转变换，从而扩大训练集。**注意生成的新数据一定要和给定数据独立同分布。**

- regularization

- validation













