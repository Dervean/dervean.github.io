---
layout: post
title: "机器学习基石第6课：Theory of Generalization"
author: Dervean
description: "机器学习基石第6课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/12/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Preview

上节课指出对于无穷多个 hypothesis 的情况下我们可以使用 **$m_H$** 代替 $M$，并介绍了什么是成长函数以及 break point。

本节课是证明**存在 break point 时，成长函数满足多项式增长，机器学习是可行的**。

# Restriction of Break Point

从上节课的小例子我们得出对于 2D 的 perceptron 的 break point = 4，即当有 4 个点的时候我们没有办法找到足够多种的 dichotomy ($< 2^4$)来 shatter 这 4 个点。

本节课通过一个简单的例子来说明 break point 对成长函数增长的影响。

- 首先，对于一个点我们肯定可以找到两种 dichotomy 使得这个点要么是 +1 要么是 -1，即 $m_H(N) \ge 2$ 

- 假设我们没办法 shatter 住任意两个点，即 minimum break point k = 2，当 N = 2 的时候，$m_H(N) < 4$，不妨令 $m_H(N) = 3$。

- 基于上面两个条件，我们现在来看 N = 3，k = 2 最多有多少个 dichotomy.

  - 首先，在不 shatter 任意两个点的前提下，肯定能存在 1 个 dichotomy.

  ![1-dichotomy](/images/ML/theory-of-generalization-1.png "1 dichotomy")

  - 存在 2 个dichotomy.

  ![2-dichotomy](/images/ML/theory-of-generalization-2.png "2 dichotomy")

  - 存在 3 个dichotomy.

  ![3-dichotomy](/images/ML/theory-of-generalization-3.png "3 dichotomy")

  - 现在我们来研究是否存在 4 个 dichotomy. 首先，确实存在 4 个 dichotomy 且能满足不会 shatter 任意两个点。

  ![4-dichotomy-no-shatter](/images/ML/theory-of-generalization-4.png "不 shatter 任意两个点，存在 4 个 dichotomy")

  - 但也会存在 4 个 dichotomy 但是 shatter 其中两个点的情况。

  ![4-dichotomy-shatter](/images/ML/theory-of-generalization-5.png "4 个 dichotomy，shatter 其中两个点")





















