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

机器学习从已知的数据$D$中找到能最接近$f$的$g$，但因为$f$是未知的，找到的$g$也没办法判断其优劣，即使在部分数据上满足$g \approx f$，也不能保证在所有数据上符合条件。

那机器学习最终真的学到东西了吗？

如果想要保证机器从已知的数据中学习到东西，必须保证一个前提：**数据集（包括训练集和测试集）必须是从相同分布中产生的。**

机器学习是从**样本数据（sample）中学习到数据的分布。**

![feasibility](/images/ML/feasibility-1.png "feasibility")

# [Hoeffding Inequality](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)

课程中给出一个例子来引出$Hoeffding Inequality$：

有一个罐子，罐子里面有许许多多的橘色和绿色的弹珠，我们无法将所有弹珠都数一遍，如何知道**橘色弹珠的比例**。

这时候可以利用**抽样**来估计，为了减少误差，需要增大抽样的样本数目$**N**$。

假如橙色弹珠的真实比例为$\mu$, 而从样本中估计出的比例为$\nu$，样本大小$**N**$，$\epsilon$表示误差范围:

$$P(\mid \nu − \mu \mid > \epsilon) \le 2exp(−2 (\epsilon)^2 N)$$

其中推论$\mu = \nu$是符合$PAC$理论的(**probably approximately correct**，即大概率可以认为是正确的)。

# 从罐子到机器学习

我们来做个类比：

绿色弹珠 $Leftrightarrow$ 模型$h$预测结果与样本真实数据一致.

橘色弹珠 $Leftrightarrow$ 模型$h$预测结果与样本真实数据不一致.

橘色弹珠的比例即模型$h$的错误率.

样本中的橘色弹珠的比例为$E_{in}(h)$.

全部弹珠中的橘色弹珠的比例为$E_{out}(h)$.

根据$Hoeffding Inequality$：

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2exp(−2 (\epsilon)^2 N)$$

可以看出: $N$越大，$E_{in}(h)$ 和 $E_{out}(h)$， 也就是训练误差和泛化误差越接近.

$E_{in}(h) \approx E_{out}(h)$ **and** $E_{in}(h)$**small** $\Rightarrow$ $E_{out}(h)$**small** $\Rightarrow$ $h \approx f$ with respect to P












































