---
layout: post
title: "机器学习基石第10课：Logistic Regression"
author: Dervean
description: "机器学习基石第10课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/19/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Logistic Regression Problem

前面的课程中已经介绍了二分类问题和线性回归问题，那逻辑回归（Logistic Regression Problem）又是什么呢？

例子：假设现在要判断一个人是否患有心脏病，这感觉像是一个二分类问题（患有心脏病，没有心脏病）:

$$f(x) = sign(P(+1 | x)-\frac{1}{2}) \in \{-1,+1\}$$

在学习二分类问题的时候我们已经知道，在有噪声数据的影响下，判断的结果可能并不对，但给出的结果只有 “+1” 和 "-1"，并没有其他信息告诉我们结果的**准确率**有多高。而逻辑回归就可以告知“结果正确的概率”:

$$f(x) = P(+1 | x) \in [0,1]$$

如何得出“结果正确的概率”呢？

我们的训练数据只有历年来所有病人的基本信息（年龄，血压，体重等）$x = (x_1,x_2,...,x_d)$ 和是否患有心脏病（是，否）。

对于一个给定基本信息的病人，可能只有上帝才知道他患有心脏病的几率，但是我们可以通过医学上的研究来判断患心脏病与某一项信息的相关性（例如年龄越大患心脏病几率越高，血压越高患心脏病几率越高等等），所以先计算一个加权分数:

$$s = \sum_{i=0}^{d} w_ix_i$$

这种分数计算只考虑线性加权，方式略粗糙也欠缺科学，但先假设“分数越高得病的几率越高”。

使用 S 型函数将加权分数 map 到 [0,1] 区间内（用来估计概率）:

![logistic-regression-1](/images/machine-learning-foundations/logistic-regression-1.png "sigmoid 函数")

$$\theta(s) = \frac{e^s}{1+e^s} = \frac{1}{1+e^{-s}}$$

该函数有如下性质:

$$\theta(-\infty) = 0$$

$$\theta(+\infty) = 1$$

$$\theta(0) = \frac{1}{2}$$

$$\theta(-s) = 1 - \theta(s)$$

我们使用 S 型函数来设计 hypothesis 来估计 target function:

$$h(x) = \theta (w^Tx) = \frac{1}{1+exp(-w^Tx)}$$

# Logistic Regression Error

如何计算逻辑回归中的 $E_{in}(w)$ ？

课件中使用的方式是 “likelihood”，也就是“似然估计”。

对于 target funtion，我们计算 $f(x) = P(+1 \| x)$，也就是“患病的几率”，那么不患病的几率就是 $1 - f(x)$:

$$
target \ function \ f(x) 
= P(+1 | x) \ \ \Longleftrightarrow \ \ P(y | x) 
= \left\{ {f(x) \ \ \ \ \ \ \ \ \ for \ y = +1 \atop 1-f(x) \ \ for \ y = -1} \right.
$$

对于训练集 $\mathcal{D} = \{(x_1,o),(x_2,x),...,(x_N,x)\}$，其中 $o$ 表示正例（$y = +1$），$x$ 表示负例（$y = -1$），目标函数 $f$ 产生 $\mathcal{D}$ 的概率为 

$$P(x_1)P(o \|x_1) \times P(x_2)P(x \|x_2) \times ... \times P(x_N)P(x \|x_N)$$

即 

$$P(x_1)f(x_1) \times P(x_2)(1-f(x_2)) \times ... \times P(x_N)(1-f(x_N))$$

其中 $P(x_1), P(x_2),..., P(x_N)$ 已知，使用 $h(x)$ 来估计 $f(x)$:

$$
\begin{array}{rcl}
likelihood(h) & \approx 	& P(x_1)h(x_1) \times P(x_2)(1-h(x_2)) \times ... \times P(x_N)(1-h(x_N)) \\
			  &		=		& P(x_1)h(x_1) \times P(x_2)h(-x_2) \times ... \times P(x_N)h(-x_N)  \\
			  &		=		& P(x_1)P(x_2)...P(x_N) \prod_{n=1}^{N} h(y_n x_n) \\
			  &	\varpropto	& \prod_{n=1}^{N} h(y_n x_n)
\end{array}
$$

为了更加接近实际概率值，这个估计概率值应该越大越好:

$$
\begin{array}{rl}
& \underset{h}{max} \ likelihood(logistic \ h) \varpropto \prod_{n=1}^{N} h(y_n x_n) \\
\Rightarrow  & \underset{w}{max} \ likelihood(w) \varpropto \prod_{n=1}^{N} \theta(y_nw^Tx_n) \\
\Rightarrow  & \underset{w}{max} \ \sum_{n=1}^{N} \ln\theta(y_nw^Tx_n) \\
\Rightarrow  & \underset{w}{min} \ \sum_{n=1}^{N}- \ln\theta(y_nw^Tx_n) \\
\Rightarrow  & \underset{w}{min} \ \frac{1}{N}\sum_{n=1}^{N}- \ln\theta(y_nw^Tx_n) \\
& (\theta(s) = \frac{1}{1+exp(-s)}) \\
\Rightarrow  & \underset{w}{min} \ \frac{1}{N}\sum_{n=1}^{N} \ln(1 + exp(-y_nw^Tx_n)) \\
\Rightarrow  & \underset{w}{min} \ \underbrace{\frac{1}{N}\sum_{n=1}^{N}err(w,x_n,y_n)}_{E_{in}(w)}
\end{array}
$$

至此我们定义 $E_{in}(w) = \frac{1}{N} \sum_{n=1}^N err(w,x_n,y_n)$，其中 $err(w,x_n,y_n) = \ln(1 + exp(-y_nw^Tx_n))$，也称为 **"Cross-Entropy Error"**（交叉熵误差）。

# Gradient of Logistic Regression Error

如何 minimize $E_{in}(w)$ ？

$E_{in}(w)$ 是关于 $w$ 的连续的一阶可导且二阶可导的凸函数（continuous，differentiable，twice-differentiable，convex），具有局部最小点:

$$
\triangledown E_{in}(w) = \frac{1}{N} \sum_{n=1}^N (\theta (-y_nw^Tx_n))(-y_nx_n)
$$

令 $\triangledown E_{in}(w) = 0$，即:

$$\frac{1}{N} \sum_{n=1}^N (\theta (-y_nw^Tx_n))(-y_nx_n) = 0$$

注意到：如果所有的 $\theta (-y_iw^Tx_i) = 0$，因为在 S 型函数中，当 $x = -\infty$ 时 $\theta(x) = 0$，所以 $y_iw^Tx_i \gg 0$，也意味着 $y_i$ 和 $w^Tx_i$ 同号，即训练集 $\mathcal{D}$ 是线性可分的。

如果**训练集不是线性可分的**，即并不是所有的 $\theta (-y_iw^Tx_i) = 0$，那我们还能给出解的具体函数形式（即计算出一个 closed-form solution）吗 ？—— 可惜的是，不能。

当然我们还可以有别的方法。

回顾在解决训练集非线性可分的 PLA 问题，我们使用的是 Pocket Algorithm:

1. start from some $w_0$ (say 0), and 'correct' its mistakes on $\mathcal{D}$.

2. find a mistake of $w_t$ called $(x_{n(t)},y_{n(t)})$.

   $$sign(w_t^T x_{n(t)}) \neq y_{n(t)}$$

3. (try to) correct the mistake by
  
   $$w_{t+1} \ \leftarrow \ w_t + y_{n(t)}x_{n(t)}$$

4. loop step 2 and step 3 until find a best solution $w$ as g.

采取的方法是**迭代优化**（iterative optimization），即不断比较‘袋子’里的解和当前的解，并将更优秀的解放进‘袋子’里。

以上的 step 2 和 step 3 其实可以合并为 1 个 step:

$$
w_{t+1} \ \leftarrow \ w_t + [\![ sign(w_t^T x_n) \neq y_n ]\!] y_nx_n
$$

进一步可以写成:

$$w_{t+1} \ \leftarrow \ w_t + \underbrace{1}_{\eta} \cdotp \underbrace{[\![ sign(w_t^T x_n) \neq y_n ]\!] y_nx_n }_{v}$$

其中 $\eta$ 为**学习步长**，$v$ 为**学习方向**，

$$\text{choice of } (\eta, v) \text{ and stopping condition defines iterative optimization approach.}$$

下面我们使用**迭代优化方法（iterative optimization approach）** 来 minimize $E_{in}(w)$。

# Gradient Descent

梯度下降是一种迭代优化方法，沿着方向 $v$（指示方向，一般使用单位向量）以 $\eta$（一般使用正数）的步长滑到谷底:

![logistic-regression-2](/images/machine-learning-foundations/logistic-regression-2.png "Gradient Descent")

我们需要 minimize $E_{in}(w)$:

$$\underset{\parallel v \parallel = 1}{min} \ E_{in}(w_t + \eta v)$$

因为一般 $\eta$ 都给定且比较小，所以可以使用泰勒展开:

$$
\underset{\parallel v \parallel = 1}{min} \ E_{in}(w_t + \eta v) 
\approx \underset{\parallel v \parallel = 1}{min} \ \underbrace{E_{in}(w_t)}_{\text{known}} + \underbrace{\eta}_{\text{given positive}} v^T \underbrace{\triangledown E_{in}(w_t)}_{\text{known}}
$$

上面式子中只有 $v^T$ 是未知的，即我们已经从 $E_{in}(w)$ 转化成 $E_{in}(v)$。

为了使上述式子最小，只需  **$v^T$ （是一个单位向量）的方向和 $\triangledown E_{in}(w_t)$ 的方向相反** :

$$ v = - \frac{\triangledown E_{in}(w_t)}{\parallel \triangledown E_{in}(w_t) \parallel} $$

Gradient Descent:

$$
for \ small \ \eta, \ w_{t+1} \leftarrow w_t - \eta \frac{\triangledown E_{in}(w_t)}{\parallel \triangledown E_{in}(w_t) \parallel}.
$$

这里有一个问题：我们始终将 $v$ 设为单位向量，即 $\parallel \eta v \parallel$ 永远都是定值 $\eta$，而如果 $\eta$ 太小，收敛（下降）得太慢；如果 $\eta$ 太大则可能会导致无法收敛。

![logistic-regression-3](/images/machine-learning-foundations/logistic-regression-3.png "步长太小、步长太大、步长不断调整")

我们希望 $\parallel \eta v \parallel$ 能够随情况改变而改变，既能够快速收敛，但也不至于在低谷周围“震荡”，当 $\triangledown E_{in}(w_t) \uparrow$，$\parallel \eta v \parallel \uparrow$；当 $\triangledown E_{in}(w_t) \downarrow$，$\parallel \eta v \parallel \downarrow$:

$$w_{t+1} \leftarrow w_t - \eta \triangledown E_{in}(w_t)$$

综上所述，完整的 logistic regression 算法:

1. initializa $w_0$.

2. compute 

   $$ \triangledown E_{in}(w) = \frac{1}{N} \sum_{n=1}^N (\theta (-y_nw^Tx_n))(-y_nx_n)$$

3. update by

   $$ w_{t+1} \leftarrow w_t - \eta \triangledown E_{in}(w_t) $$

4. loop step 2 and step 3 until $\triangledown E_{in}(w) = 0$ or enough iterations.

5. return last $w_{t+1}$ as $g$.





































