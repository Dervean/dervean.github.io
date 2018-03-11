---
layout: post
title: "机器学习基石第1课：The Learning Problem"
author: Dervean
description: "机器学习基石第1课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/01/28/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Key Essence

* Exists some underlying pattern to be learned.
* No programmable definition.
* There is data about the pattern.

# Formalize the learning problem

* training examples
* target function
* learning algorithm
* hypothesis set
* hypothesis

以信贷公司发放贷款为例：
* 信贷公司拥有每个用户的数据：例如用户的年龄、每月收入以及欠款额度等信息，记作$X$；
* 决定是否发放贷款，记作$Y$；
* target function：$f: X \rightarrow Y$，$f$是无法 programmable definited 的，所以没法知道到底是什么；
* training examples：信贷公司存在一些历史数据$D={(x_1,y_1),(x_2,y_2),...,(x_n,y_n)}$；
* learning algorithm：此时通过一个学习算法，从历史数据中学习；
* hypothesis set：学习后得到许多函数$g: X \rightarrow Y$，且都是可以 programmable definited 的，这些函数是大部分甚至全部满足历史数据$D$的；
* hypothesis：尽可能得减小和真实函数$f$的差距，需要从 hypothesis set 里选一个最好的 hypothesis。

![definition](/images/machine-learning-foundations/definition-ML.png "definition")