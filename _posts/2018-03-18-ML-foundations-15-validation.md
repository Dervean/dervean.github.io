---
layout: post
title: "机器学习基石第15课：Validation"
author: Dervean
description: "机器学习基石第15课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/03/18/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Summary

本节课主要介绍了为什么在机器学习中要把所有数据分为训练集与测试集，解释了该做法的理论依据。

# Model Selection Problem 

为了达到 $min(E_{out})$ 的目的，可以选择不同的算法，选择不同的学习速率，选择不同的迭代次数、选择各式各样的特征转换或者 regularizer，然而怎么才能判断得到的 model 是最优的。

肯定不能根据模型的 $E_{in}$ 选择，因为 $E_{in}$ 最小的时候会造成过拟合的问题，模型泛化能力差。

可是我们也不知道这个模型用到现实情况下怎么样，好比天天做模拟卷，谁也不知道高考出啥题。

# Validation

解决的方式就是将现有数据划分为训练集和测试集（validation set）：现在有 $N$ 个数据，将其中的 $N-K$ 个数据用于训练得到 $g_m^-$，另外 $K$ 个数据用于测试并计算 $E_{val}$。

为了保证独立同分布，选取的时候需要随机抽样。根据 finite-bin Hoffding 不等式:

$$
E_{out}(g_m^-) \le E_{val}(g_m^-) + O\left(\sqrt{\frac{\log{M}}{K}} \right)
$$

此时只需要选择能使 $E_{val}$ 最小的模型即可，换言之，选择在测试集上表现最好的模型:

$$
m^{\ast} = \underset{1 \le m \le M}{argmin}(E_m = E_{val}(\mathcal{A}_m(D_{train})))  
$$

但是因为剔除了一部分数据用来测试，则用来训练的数据量变少，根据 learning curve，得到的模型相比较使用所有数据所得到的模型准确率要低一些:

$$
E_{out}\left(\underbrace{g_{m^{\ast}}}_{\mathcal{A}_{m^{\ast}}(\mathcal{D})}\right) \le E_{out}\left( \underbrace{g_{m^{\ast}}^{-}}_{\mathcal{A}_{m^{\ast}}(\mathcal{D}_{train})} \right)
$$

所以为了解决这个问题，在得到模型 $g_{m^{\ast}}^{-}$ 后，还需要将测试集加入训练数据重新得到最终的模型 $g_{m^{\ast}}$。

![validation-1](/images/machine-learning-foundations/validation-1.png "流程图")

测试集不能太小，太小的话无法保证 $E_{out}(g^-) \approx E_{val}(g^-)$；

测试集也不能太大，因为总数据量一定，测试集大会导致训练集少，训练效果不佳，即不能保证 $E_{out}(g^-) \approx E_{out}(g)$。

一般取 $K = \frac{N}{5}$。

# Leave-One-Out Cross Validation 

留一交叉验证是 validation 一个 extreme case: $K = 1$。即测试集就只有一个数据，训练集是剩下的 $N-1$ 个数据。

假设第 $n$ 个数据被用于测试，设 $E_{val}(g_n^-) = err(g_n^-(x_n),y_n) = e_n$，则 $e_n$ 的期望值为:

$$
\begin{array}{rcl}
E_{loocv}(\mathcal{H},\mathcal{A})	& = & \frac{1}{N}\sum_{n=1}^{N}e_n \\
									& = & \frac{1}{N} \sum_{n=1}^{N}err(g_n^-(x_n),y_n)
\end{array}
$$

其中 loocv 代表 Leave-One-Out Cross Validation。

可以证明 $E_{loocv}(\mathcal{H},\mathcal{A})$ 是 $E_{out}(g)$ 的无差估计:

$$
\begin{array}{rcl}
\underset{\mathcal{D}}{\mathcal{E}}E_{loocv}(\mathcal{H},\mathcal{A}) & = & \underset{\mathcal{D}}{\mathcal{E}}\frac{1}{N}\sum_{n=1}^{N}e_n \\
& = & \frac{1}{N}\sum_{n=1}^{N}\underset{\mathcal{D}}{\mathcal{E}}e_n \\
& = & \frac{1}{N}\sum_{n=1}^{N}\underset{\mathcal{D}}{\mathcal{E}}\underset{(x_n,y_n)}{\mathcal{E}}err(g_n^-(x_n),y_n) \\
& = & \frac{1}{N}\sum_{n=1}^{N}\underset{\mathcal{D}}{\mathcal{E}}E_{out}\left(g^-\right) \\
& = & \frac{1}{N}\sum_{n=1}^{N}\overline{E_{out}}\left(N-1\right) \\
& = & \overline{E_{out}}\left(N-1\right)
\end{array}
$$

# V-Fold Cross Validation

留一交叉验证法有两个比较大的弊端，一是计算量太大，$N$ 个数据就要训练 $N$ 次模型，二是稳定性不好，没法适应一些问题，例如二分类问题，取值只有 0、1，不确定性太强。

不过留一交叉验证法却提供给我们一个思路：数据集可以分成多块，每次取其中任意一块作为测试集，其他的作为训练集。

这种方法称为 V-Fold Cross Validation，实际中该方法比留一交叉验证常用。









