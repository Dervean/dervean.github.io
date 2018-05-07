---
layout: post
title: "机器学习基石第16课：Three Learning Principles"
author: Dervean
description: "机器学习基石第16课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/05/07/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# Occam's Razor

$$
\text{“在一堆可以解释已知数据的模型当中，选择最简单的模型。”}
$$

# Sampling Bias

$$
\text{“尽量使训练数据和验证数据服从同一分布。“}
$$

例如在电影推荐问题中，选择时间更近的数据作为验证集更好。

# Data Snooping

$$
\text{“训练模型时避免因为\“偷看数据\”而人为改变模型。“}
$$

例如: 

某个人发了一篇论文对数据 $\mathcal{D}$ 建模 $\mathcal{H}_1$。

第二个人看了这篇论文，对模型 $\mathcal{H}_1$ 做“改进”，在此基础上又建立模型 $\mathcal{H}_2$。

第三个人又建立模型 $\mathcal{H}_3$。

这也是偷窥数据的例子，虽然后人的模型可能越来越“好”，但很有可能只是 “overfitting” / “bad generalization”。















