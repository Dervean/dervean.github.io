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

$$ h(x) = w_T x $$

误差度量 (Error Measure) 使用 squared error ($err(\hat{y} , y) = (\hat{y} - y)^2$):

$$E_{in}(hw) = \frac{1}{N} \sum_{n=1}^{N} (\underbrace{h(x_n)}_{w_T x_n} - y_n)^2$$

$$E_{out}(w) = \underset{(x,y) \sim P}{\varepsilon} (w_Tx - y)^2$$

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

![linear-regression-1](/images/machine-learning-foundations/linear-regression-1.png "continuous, differentiable, convex)")

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

# Generalization Issue

这里讨论

# Linear Regression for Binary Classification




























