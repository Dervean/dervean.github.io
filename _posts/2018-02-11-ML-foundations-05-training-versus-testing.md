---
layout: post
title: "机器学习基石第5课：Training versus Testing"
author: Dervean
description: "机器学习基石第5课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/11/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Preview

上节课已经得出结论:

**如果所有数据都满足同一分布，模型的hypothesis的个数 $M$ 是有限的，样本数目 $N$ 足够大，则能保证 $E_{in}(h) \approx E_{out}(h)$，此时如果再能找到一个$E_{in}(h) \approx 0$，则$E_{out}(h) \approx 0$，从而实现机器学习。**

即在 multiple hypothesis 的情况下:

$$P(\mid E_{in}(h) − E_{out}(h) \mid > \epsilon) \le 2 \cdotp M \cdotp exp(−2 \epsilon^2 N)$$

其中有一个限制条件是: $M$ 是有限的. 那如果 $M$ 是无限的情况该怎么考虑，机器学习是否还可行？ 

$$
\begin{array}{rcl}
\mathbb{P}_D(Bad \ D) 	 & 			= 		& \mathbb{P}_D(Bad \ D \ for \ h_1 \ or \ Bad \ D \ for \ h_2 \ or \ ... \ or \ Bad \ D \ for \ h_M) 	\\
						 & 			\le 	& \mathbb{P}_D(Bad \ D \ for \ h_1) + \mathbb{P}_D(Bad \ D \ for \ h_2) + ... + \mathbb{P}_D(Bad \ D \ for \ h_M)		\\
						 &	(union \ bound) &								\\
						 & 			\le 	& 2exp(−2 \epsilon^2 N) + 2exp(−2 \epsilon^2 N) + ... + 2exp(−2 \epsilon^2 N) \\
						 & 			=		& 2 \cdotp M \cdotp exp(−2 \epsilon^2 N)	
\end{array}
$$
















