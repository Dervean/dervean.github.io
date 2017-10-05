---
layout: post
title: "Basic operation of Latex"
description: "A quick start with Latax"
categories: [latex]
tags: [latex]
redirect_from:
  - /2017/10/05/
---

卜神算法作业要求Latex.先熟悉一下Latex基本操作.

参考[LaTeX学习视频-西北农林科技大学-耿楠]("http://www.bilibili.com/video/av10360699/").

---

## 环境

Tex Live + TeXstudio

## 文件基本结构

	%导言区
	\documentclass{article} %report,book,letter

	\title{First}
	\author{Dervean}
	\date{\today}

	%正文区
	\begin{document}
		\maketitle
		
		Hello,  \LaTeX .
		
	\end{document}

