---
layout: post
title: "机器学习基石：什么时候该使用机器学习"
author: Dervean
description: "机器学习基石第一课"
categories: [machine learning]
tags: [ML]
redirect_from:
  - /2018/01/28/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

###什么时候该使用机器学习

* Exists some underlying pattern to be learned.
* No programmable definition.
* There is data about the pattern.

###Formalize the learming problem

* training examples
* target function
* learning algorithm
* hypothesis set
* hypothesis

以信贷公司发放贷款为例：
    信贷公司拥有每个用户的数据：例如用户的年龄、每月收入以及欠款额度等信息，记作X；
    决定是否发放贷款，记作Y；
    target function：**f**是X到Y的函数，该函数是无法 programmable definited的，所以没法知道到底是什么；
    training examples：信贷公司存在一些历史数据{(X,Y)}；
    learning algorithm：此时通过一个学习算法，从历史数据中学习；
    hypothesis set：学习后得到许多从X到Y的函数，且都是可以programmable definited的，这些函数是大部分甚至全部满足历史数据{(X,Y)}的；
    hypothesis：尽可能得减小和真实函数**f**的差距，需要从hypothesis set里选一个最好hypothesis。

![Definition](https://github.com/Dervean/dervean.github.io/tree/master/images/ML/definition-ML.png)