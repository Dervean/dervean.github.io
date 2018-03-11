---
layout: post
title: "机器学习基石第3课：Types of Learning"
author: Dervean
description: "机器学习基石第3课"
categories: [machine_learning]
tags: [machine_learning]
redirect_from:
  - /2018/02/02/
---

[机器学习基石 - 林轩田](https://www.csie.ntu.edu.tw/~htlin/course/mlfound17fall/)

---

* Kramdown table of contents
{:toc .toc}

# 根据输出空间分类

* binary classification: $Y=\\{-1,+1\\}$
* multiclass classification: $Y=\\{1,2,\cdots,K\\}$
* regression: $Y = \mathbb{R}$
* structured learning: $Y= structures$, 例如自然语言处理中对一句话中的每个词进行词性分析，此时一个句子就是一个$stucture$.

# 根据样本是否标记分类

* supervised: all $y_n$
* unsupervised: no $y_n$
* semi-supervised: some $y_n$
* reinforcement: implicit $y_n$ by goodness($\tilde{y_n}$)

# 根据训练方法分类

* batch: learn from **all** known data.
* online: hypothesis ‘improves’ through receiving data instances **sequentially**.
* active learning: improve hypothesis with fewer labels (hopefully) by **asking questions strategically**.

# 根据输入空间分类

* concrete: 输入的样本已经明确给出了其各种特征，如信用卡例子中顾客的各项资料等。
* raw: 特征不明显，但有明显的物理意义，需要经过处理才能得到特征，如字迹识别，能用来处理的是墨水的“密度”、“对称度”等等。
* abstract: 没有任何物理意义，如电影推荐，但数据却只有用户和电影的ID.



