---
layout: post
title: "机器学习基石第9课：Linear Regression"
author: Dervean
description: "机器学习基石第9课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/18/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Linear Regression Problem

还是以信贷公司为例，只不过这次不是判断能否发放贷款（Linear Classification），而是计算发放贷款额度（Linear Regression）。

用户的个人信息（年收入，工作时间，负债...）记为 $(x_1,x_2,x_3,...,x_d)$，另外加一个常数项 $x_0$，则把 $x = (x_0,x_1,x_2,x_3,...,x_d)$ 记为**“特征“**（features of customer）。

现在希望计算能够衡量各个信息的权重 $w_i$，从而使得发放贷款额度 y:

$$y \approx \sum_{i=0}^d w_i x_i$$

从而引出 hypothesis:

$$ h(x) = w^T x $$

误差度量 (Error Measure) 使用 squared error ($err(\hat{y} , y) = (\hat{y} - y)^2$):

$$E_{in}(hw) = \frac{1}{N} \sum_{n=1}^{N} (\underbrace{h(x_n)}_{w^T x_n} - y_n)^2$$

$$E_{out}(w) = \underset{(x,y) \sim P}{\varepsilon} (w^Tx - y)^2$$

如何 minimize $E_{in}$？

# Linear Regression Algorithm

$$
\begin{array}{rcl}

E_{in}(w) & = & \frac{1}{N} \sum_{n=1}^{N} (w^T x_n - y_n)^2  			\\
          & = & \frac{1}{N} \sum_{n=1}^{N} (x_n^T w - y_n)^2			\\
          & = & \frac{1}{N} {\left\|   
							\begin{matrix} 
							x_1^T w - y_1 \\ 
							x_2^T w - y_2 \\ 
							... 		  \\ 
							x_N^T w - y_n 
							\end{matrix}   
							\right\|}^2						\\
		  & = & \frac{1}{N} {\left\|   
		  					{\left[
							\begin{matrix} 
							-- x_1^T -- \\ 
							-- x_2^T -- \\ 
							... 		\\ 
							-- x_N^T --  
							\end{matrix}   
							\right]}
							w-
							{\left[
							\begin{matrix} 
							y_1 \\ 
							y_2 \\ 
							... \\ 
							y_N  
							\end{matrix}   
							\right]}
							\right\|}^2						\\
		  & = & \frac{1}{N} {\left\| \underbrace{X}_{N \times (d+1)} \underbrace{w}_{(d+1) \times 1} - \underbrace{y}_{N \times 1} \right\|}^2					
\end{array}
$$

$$
\underset{w}{min} \ E_{in}(w) 	= \frac{1}{N} {\left\| Xw - y\right\|}^2 
								= \frac{1}{N} ((Xw)^T X w - 2 (Xw)^T y + y^T y)
$$

$E_{in}(w)$ 是一个连续可导凸函数 (continuous, differentiable, convex):

![linear-regression-1](/images/machine-learning-foundations/linear-regression-1.png "continuous, differentiable, convex")

所以可以找到一个 best w 使得:

$$
\triangledown E_{in}(w) = 	\left[ 
								{\begin{matrix} 
									\frac{\partial E_{in}}{\partial w_0} (w) \\ 
									\frac{\partial E_{in}}{\partial w_1} (w) \\ 
									... 		\\ 
									\frac{\partial E_{in}}{\partial w_d} (w)   
								\end{matrix}
							}\right]
						=	\left[ 
								{\begin{matrix} 
									0 \\ 
									0 \\ 
									... 		\\ 
									0 \\   
								\end{matrix}
							}\right]
$$

结合上面的 $E_{in}(w)$:

$$\triangledown E_{in}(w) = \frac{2}{N} (X^T X w - X^Ty) = 0$$

解得:

$$w_{LIN} = (X^T X)^{-1} X^T y$$

这时候我们就可以使用 $w_{LIN}$ 来做一些 prediction 了:

$$\hat{y} = X w_{LIN} = X (X^T X)^{-1} X^T y$$

# Generalization Issue

这里讨论了 $E_{in}$ 的物理意义，主要是线性代数中 $I - X (X^T X)^{-1} X^T$ 与投影间的关系，不再赘述。

**Learning Curve**:

$$ \overline{E_{out}} = noise \ level \ \cdotp (1 + \frac{d+1}{N})$$

$$ \overline{E_{in}} = noise \ level \ \cdotp (1 - \frac{d+1}{N})$$

![linear-regression-2](/images/machine-learning-foundations/linear-regression-2.png "Learning Curve")

# Linear Regression for Binary Classification

现在已经通过信贷公司的例子介绍了 Linear Classification 和 Linear Regression，现在我们看一下二者的不同:

* Linear Classification vs. Linear Regression

	|---
	| Linear Classification | Linear Regression
	|:-|:-
	| $\mathcal{y} = \{-1,+1 \}$ | $\mathcal{y} = \mathbb{R}$
	| $h(x) = sign(w^T x)$ | $h(x) = w^T x$
	| $err(\hat{y},y) = [\![ \hat{y} \neq y ]\!]$ | $err(\hat{y},y) = (\hat{y} - y)^2$
	| NP-hard | efficient analytic solution

既然线性回归比线性分类好解，那我们是否可以使用线性回归来帮助解线性分类的问题呢？

先证明: $err_{0/1} \neq err_{sqr}$

![linear-regression-3](/images/machine-learning-foundations/linear-regression-3.png "Linear Classification vs. Linear Regression")

$$upper \ bound \ err_{sqr} \ as \ \hat{err} \ to \ approximate \ err_{0/1}.$$

既然 $err_{sqr}$ 可以作为 $err_{0/1}$ 的上限，那么 $w_{L
IN}$ 就可以作为 Linear Classification 的 baseline，在 Pocket Alogarithm 里初始化口袋里的 $w_0 = w_{LIN}$，从而加速分类的进程。

























